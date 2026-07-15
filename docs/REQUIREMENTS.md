# Requirements

## Functional requirements

### FR-001 Offline analysis
The system shall process WAV recordings and export:
- original signal,
- estimated persistent-noise signal,
- residual signal,
- metrics and spectrogram data.

### FR-002 RSVD noise model
The system shall build a rolling complex-STFT matrix and compute a configurable low-rank approximation using deterministic RSVD.

### FR-003 Component qualification
The system shall not equate every leading singular component with noise. Components must pass low-frequency, persistence, stability, coherence, and energy tests.

### FR-004 Baseline comparison
The test harness shall compare RSVD against full truncated SVD, band filtering, adaptive notch, and at least one spectral estimator.

### FR-005 Android analysis mode
The app shall capture microphone data, analyze it on-device, and display dominant persistent bands, model rank, stability, and confidence.

### FR-006 Safe experimental output
Anti-noise output shall require explicit opt-in, supported wired route, successful calibration, limiter, watchdog, and immediate mute control.

### FR-007 Privacy
Raw microphone audio shall stay on device unless the user explicitly exports a recording.

## Non-functional requirements

### NFR-001 Real-time safety
The audio callback shall avoid allocation, blocking, logging, file I/O, JNI, and matrix decomposition.

### NFR-002 Determinism
Offline tests and RSVD benchmarks shall use fixed seeds and reproducible fixtures.

### NFR-003 Portability
Core DSP shall build and run on desktop host systems independently of Android.

### NFR-004 Measurability
Every algorithm shall expose timing, memory, quality, and stability metrics.

### NFR-005 Graceful degradation
When route latency, jitter, confidence, or model stability is unacceptable, the system shall disable generated cancellation output.
