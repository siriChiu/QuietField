# Software architecture

## Architectural principle

Separate the system into a slow analytical path and a hard real-time path.

```text
Microphone / WAV input
        |
        v
Input conditioning and low-frequency branch
        |
        +----------------------------+
        |                            |
        v                            v
Real-time path                 Analysis path
small audio blocks             rolling STFT frames
prediction/equalization        RSVD/subspace update
limiter and output             component qualification
        ^                            |
        |        immutable model     |
        +----------------------------+
```

RSVD never runs in the audio callback. The analysis worker periodically publishes an immutable `NoiseModelSnapshot`. The fast path consumes the most recent valid snapshot.

## Layered architecture

```text
Android UI / lifecycle
  - Compose screens
  - permissions
  - route diagnostics
  - session controls
  - result viewer

Application services
  - SessionController
  - DeviceCapabilityService
  - CalibrationCoordinator
  - ExportService

Audio platform layer
  - Oboe stream management
  - full-duplex synchronization
  - route change handling
  - timestamp/latency capture

DSP orchestration
  - input conditioning
  - analysis queue
  - model publication
  - prediction controller
  - output safety chain

DSP primitives
  - filters and windows
  - FFT/STFT/iSTFT
  - RSVD/full SVD
  - component scoring
  - predictors
  - delay/path correction
  - limiter

Diagnostics and testing
  - WAV laboratory
  - synthetic generators
  - benchmarks
  - metrics
  - trace export
```

## Threading model

### Audio callback thread

Responsibilities:
- Read input frames.
- Apply fixed-cost conditioning.
- Push a decimated or selected analysis stream to a bounded SPSC queue.
- Read latest model snapshot atomically.
- Run only bounded fast-path operations.
- Apply limiter and write output.

Forbidden:
- Heap allocation.
- Mutexes or condition variables.
- SVD, FFT plan creation, file access, logging, JNI calls.

### Analysis worker

Responsibilities:
- Drain analysis queue.
- Form complex STFT frames.
- Maintain rolling matrix.
- Compute or update RSVD.
- Score components.
- Publish model snapshot.

### Control/UI thread

Responsibilities:
- Lifecycle and permissions.
- User controls.
- Diagnostics display.
- Start/stop/calibration commands.

### Export worker

Responsibilities:
- Write WAV, JSON, CSV, and diagnostic files.
- Never share writable buffers with the callback.

## Major modules

### `dsp-core`
Platform-independent C++ library containing audio blocks, filters, FFT wrappers, STFT, RSVD, metrics, predictors, and safety DSP.

### `dsp-lab`
Host CLI for offline experiments and reproducible reports.

### `android-audio`
Oboe integration, device capabilities, route monitoring, timestamps, and full-duplex streams.

### `app-domain`
Kotlin use cases and session state.

### `app-ui`
Compose screens and diagnostics.

## Data flow: analysis-only MVP

```text
Mic -> Oboe callback -> conditioning -> SPSC queue
                                     -> analysis worker
                                     -> complex STFT matrix
                                     -> RSVD
                                     -> component scoring
                                     -> model snapshot
                                     -> UI diagnostics
```

## Data flow: later experimental cancellation

```text
Mic -> callback -> predictor using latest snapshot
                -> forecast low-frequency noise
                -> delay/path correction
                -> partial inversion
                -> limiter + watchdog
                -> wired headphone
```

## State machine

```text
Idle
  -> CheckingRoute
  -> AnalysisReady
  -> Analyzing
  -> CalibrationRequired
  -> Calibrating
  -> ExperimentalReady
  -> Cancelling
  -> FaultMuted
```

Any underrun storm, route change, invalid calibration, stale model, output over-limit, or watchdog timeout transitions to `FaultMuted`.

## Model publication

`NoiseModelSnapshot` is immutable and versioned. Publication uses atomic pointer exchange or a lock-free double buffer. The callback validates:
- version,
- timestamp,
- maximum age,
- confidence,
- supported frequency range,
- predictor readiness.

An invalid or stale snapshot yields zero generated output.
