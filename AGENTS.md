# AGENTS.md

## Mission

Build QuietCore as a measurable Android DSP research project. The project uses RSVD to model persistent low-frequency environmental noise and may later generate conservative anti-noise through wired or USB-C headphones.

## Mandatory behavior

1. Read `README.md`, `docs/REQUIREMENTS.md`, `docs/ARCHITECTURE.md`, `docs/DSP_DESIGN.md`, `docs/SAFETY.md`, and the active plan before editing code.
2. Work on only the milestone requested by the user or active execution plan.
3. Update the active plan with progress, decisions, test evidence, and unresolved risks.
4. Prefer small reviewable commits and isolated modules.
5. Never enable acoustic anti-noise by default.
6. Never bypass output limiting, watchdogs, route validation, or emergency mute logic.
7. Do not allocate memory, take locks, perform file I/O, log, call JNI, or run SVD inside the real-time audio callback.
8. Keep heavy analysis in a worker thread and exchange state through bounded lock-free or wait-free structures.
9. Every DSP change requires deterministic offline tests before device testing.
10. Treat audible quality claims as hypotheses until supported by measurements and listening tests.

## Technical constraints

- Kotlin for lifecycle, UI, permissions, persistence, and diagnostics.
- C++20 for real-time audio and DSP.
- CMake + Gradle.
- Oboe/AAudio for Android audio I/O.
- Float32 processing at 48 kHz unless a test explicitly uses another rate.
- Default callback block target: 64–192 frames, selected by device.
- MVP analysis band: 20–400 Hz; cancellation band initially 30–250 Hz.
- RSVD runs on rolling complex-STFT data outside the callback.
- Fast path uses the latest immutable model snapshot.
- No neural networks, cloud inference, or recording upload.

## Quality gates

A milestone is complete only when:

- Code builds in Debug and Release configurations.
- Unit tests and offline DSP tests pass.
- Formatting and static analysis pass.
- New behavior has documented acceptance criteria.
- Real-time safety review reports no callback allocations or blocking calls.
- Benchmark results are written to `docs/results/` or the active plan.

## Suggested commands

Once bootstrapped, maintain these commands or equivalent scripts:

```bash
./gradlew assembleDebug
./gradlew test
./gradlew lint
./gradlew connectedDebugAndroidTest
cmake --build build-host
ctest --test-dir build-host --output-on-failure
```

## Scope protection

Do not implement Bluetooth cancellation, full-system Android audio interception, speech cancellation, virtual audio routing, or production ANC claims unless a later ADR explicitly approves them.
