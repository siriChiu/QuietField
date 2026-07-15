# Ordered Codex task backlog

## M0 — Repository bootstrap
- Android app skeleton.
- C++ host DSP target.
- CI workflow.
- Formatting, linting, sanitizer configuration.

## M1 — Offline DSP laboratory
- WAV reader/writer.
- Audio block and channel layouts.
- Window functions.
- Complex STFT/iSTFT with reconstruction tests.
- Synthetic persistent-noise generator.
- Metrics and result export.

## M2 — Noise-model baselines
- Band-pass baseline.
- Adaptive notch baseline.
- Spectral subtraction baseline.
- Truncated full-SVD reference.
- RSVD implementation and benchmark.
- Rank-selection and stability scoring.

## M3 — Recorded dataset evaluation
- Record/import fan, HVAC, vehicle, computer-fan, and hum samples.
- Export original, estimated-noise, and residual WAV files.
- Generate spectrogram and metric reports.
- Compare quality, runtime, memory, and artifacts.

## M4 — Android audio foundation
- Oboe device enumeration.
- Full-duplex bypass.
- Route change handling.
- Latency/jitter diagnostics.
- Wired/USB-C eligibility checks.

## M5 — On-device analysis-only mode
- Microphone capture into analysis queue.
- Rolling complex-STFT matrix.
- Background RSVD worker.
- UI showing rank, dominant band, stability, and confidence.
- No anti-noise output.

## M6 — Predictive model
- Track low-rank coefficients.
- Compare AR, PLL, Kalman, and NLMS predictors.
- Select predictor per component.
- Forecast-horizon and jitter testing.

## M7 — Open-loop anti-noise laboratory
- Fixed calibrated delay.
- Headphone-path correction filter.
- Conservative gain and limiter.
- Emergency mute and watchdog.
- Acoustic test fixture only.

## M8 — Adaptive experimental cancellation
- Secondary-path identification.
- Error microphone support where hardware permits.
- FxLMS or hybrid controller.
- RSVD-guided component selection.
- Controlled beta testing.
