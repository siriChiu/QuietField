# Bootstrap execution plan

## Goal

Create a repository in which Codex can implement and objectively evaluate the RSVD persistent-noise model before any acoustic cancellation is attempted.

## Progress

- [ ] Android project initialized.
- [ ] Native C++ library linked through JNI.
- [ ] Host CMake target created.
- [ ] Test framework selected and configured.
- [ ] WAV fixtures supported.
- [ ] STFT/iSTFT implemented and tested.
- [ ] Benchmark/report directories created.
- [ ] CI configured.

## Milestone 0 tasks

1. Create Gradle multi-module structure:
   - `app`
   - `android-audio`
   - `dsp-native`
2. Create host CMake targets:
   - `quietcore_dsp`
   - `quietcore_lab`
   - `quietcore_tests`
3. Configure C++ warnings, sanitizers for host, Kotlin lint, Android lint, and formatting.
4. Add CI for host tests and Android build.

## Milestone 1 tasks

1. Implement non-owning audio block views and owned offline buffers.
2. Implement mono WAV PCM/float read/write sufficient for fixtures.
3. Implement window functions.
4. Add FFT backend abstraction.
5. Implement complex STFT and iSTFT.
6. Add reconstruction, impulse, sine, and random-signal tests.
7. Add synthetic noise generators:
   - fixed tone,
   - drifting tone,
   - harmonic motor,
   - low-frequency colored noise,
   - persistent noise plus speech-like signal.
8. Implement metric and JSON result structures.
9. Add a CLI command that exports original/reconstructed WAV and metrics.

## Acceptance criteria

- Host Debug and Release builds succeed.
- Android Debug build succeeds.
- STFT/iSTFT reconstruction meets documented tolerance.
- Tests are deterministic.
- Sanitizers report no issue in host tests.
- CLI creates valid result files.
- No live microphone or generated anti-noise code is enabled.

## Decisions log

Codex must append dated decisions here instead of silently changing architecture.

## Evidence log

Codex must append commands and summarized results here.
