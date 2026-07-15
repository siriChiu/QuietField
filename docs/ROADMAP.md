# Roadmap

## M0 — Foundation

Deliverables:
- Android/NDK repository.
- Host C++ DSP build.
- CI, formatting, linting, sanitizers.

Exit criteria:
- Clean build and test on host and Android emulator/device.

## M1 — Offline signal laboratory

Deliverables:
- WAV import/export.
- STFT/iSTFT.
- Synthetic noise fixtures.
- Metrics and report format.

Exit criteria:
- Near-perfect reconstruction test.
- Reproducible CLI run.

## M2 — RSVD persistent-noise model

Deliverables:
- Full truncated SVD reference.
- Deterministic RSVD.
- Rolling matrix and rank selection.
- Component qualification and reconstruction.
- Baseline comparison.

Exit criteria:
- RSVD approximation error documented.
- At least one multi-source persistent-noise case where RSVD provides a meaningful advantage or equivalent quality with lower cost.

Decision gate:
- Continue with RSVD as primary model, hybridize it, or demote it based on evidence.

## M3 — Recorded dataset study

Deliverables:
- Curated local dataset and metadata.
- Listening package.
- Quantitative report.

Exit criteria:
- Results for at least five persistent-noise environments.
- Known failure cases documented.

## M4 — Android real-time analysis

Deliverables:
- Oboe input pipeline.
- Analysis worker and model snapshots.
- Route/latency diagnostics.
- Live UI.

Exit criteria:
- No callback allocation/blocking.
- Stable 30-minute analysis session on target devices.
- Acceptable CPU and battery profile.

## M5 — Prediction research

Deliverables:
- Low-rank coefficient tracking.
- AR/PLL/Kalman/NLMS comparison.
- Forecast confidence.

Exit criteria:
- Forecast error characterized versus horizon and jitter.

## M6 — Open-loop acoustic prototype

Deliverables:
- Calibration tool.
- Fixed delay/path correction.
- Conservative anti-noise output on fixture.
- Complete safety chain.

Exit criteria:
- Repeatable target-band reduction on a coupler/dummy setup.
- No unstable amplification during fault injection.

## M7 — Adaptive ANC research

Deliverables:
- Secondary-path identification.
- Optional error microphone.
- FxLMS/hybrid controller.
- RSVD-guided component selection.

Exit criteria:
- Demonstrated benefit over predictor-only method.
- Hardware support matrix.

## M8 — Productization

Deliverables:
- Device compatibility database.
- Onboarding and calibration UX.
- Privacy review.
- Beta telemetry limited to non-audio diagnostics with explicit consent.
- Honest product claims.
