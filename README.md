# QuietCore (working title)

An Android research application for low-cost reduction of persistent low-frequency environmental noise using ordinary wired or USB-C headphones.

## Product promise

QuietCore does **not** promise full-spectrum commercial ANC. Its first goal is narrower and measurable:

> Detect, model, and reduce persistent, predictable low-frequency background noise such as fans, compressors, engines, HVAC hum, and transport rumble.

The first technical differentiator is **RSVD-based persistent-noise modeling**. RSVD identifies stable low-rank components in a rolling complex STFT representation. A separate real-time path later predicts and generates conservative anti-noise.

## Non-goals for MVP

- No masking sounds, white noise, rain sounds, or focus music.
- No cloud AI or neural-network inference.
- No claim of replacing premium ANC headphones.
- No Bluetooth ANC guarantee.
- No cancellation of speech, alarms, impacts, keyboards, or other fast transient sounds.
- No full-system processing of audio from unrelated Android apps.

## Target platform

- Android 12+ initially.
- Kotlin + Jetpack Compose for application/UI.
- C++20 through Android NDK for DSP.
- Oboe/AAudio for low-latency full-duplex audio.
- Wired 3.5 mm or USB-C headset is the supported MVP path.

## Repository map

- `AGENTS.md`: durable Codex instructions.
- `CODEX_BOOTSTRAP_PROMPT.md`: first prompt to run in Codex.
- `CODEX_TASKS.md`: ordered implementation backlog.
- `docs/PRODUCT.md`: users, positioning, and success criteria.
- `docs/REQUIREMENTS.md`: functional and non-functional requirements.
- `docs/ARCHITECTURE.md`: software and runtime architecture.
- `docs/DSP_DESIGN.md`: DSP pipeline and mathematical model.
- `docs/ANDROID_DESIGN.md`: Android-specific implementation decisions.
- `docs/API_CONTRACTS.md`: module boundaries and data contracts.
- `docs/TEST_PLAN.md`: offline, device, and acoustic validation.
- `docs/SAFETY.md`: output limits and fail-safe behavior.
- `docs/ROADMAP.md`: milestones from offline prototype to experimental ANC.
- `docs/adr/`: architectural decisions.
- `docs/plans/`: Codex execution plans.

## Development order

1. Reproducible offline DSP laboratory.
2. STFT/iSTFT and RSVD noise-model validation.
3. Compare RSVD with simpler baselines.
4. Android low-latency capture/render bypass.
5. On-device analysis-only mode.
6. Prediction and delay calibration.
7. Conservative low-frequency anti-noise experiment.
8. Closed-loop research only after safety and measurement gates pass.

## Initial success criterion

Before any acoustic cancellation is enabled, the project must demonstrate on recorded datasets that RSVD can estimate persistent low-frequency noise more cleanly than selected baselines without excessive residual artifacts.
