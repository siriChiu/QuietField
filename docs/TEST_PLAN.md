# Test plan

## Test pyramid

### Unit tests
- Window functions.
- FFT round trip.
- STFT/iSTFT reconstruction.
- Ring buffer behavior.
- RSVD matrix approximation.
- Rank selection.
- Component score and hysteresis.
- Limiter and watchdog.

### Property tests
- No NaN/Inf propagation.
- Bounded output for bounded input.
- Reconstruction error under configured tolerance.
- Deterministic output for fixed seed.
- Model publication never exposes partial state.

### Offline DSP tests

Synthetic fixtures:
- Single tone with drift.
- Harmonic motor model.
- Multiple fans.
- Low-frequency colored noise.
- Persistent noise plus speech.
- Persistent noise plus alarm/transients.

Recorded fixtures:
- Desk fan.
- HVAC.
- Desktop computer fans.
- Refrigerator/compressor.
- Bus/train interior.
- 50/60 Hz hum and harmonics.

## Baseline metrics

- Segmental SNR improvement.
- Target-band attenuation.
- Non-target-band distortion.
- Log-spectral distance.
- Noise-estimation error when ground truth exists.
- Transient preservation.
- Runtime per update.
- Peak memory.
- Model churn and rank stability.

No single metric replaces listening evaluation.

## Listening protocol

Blindly compare:
- original,
- fixed filter,
- adaptive notch,
- spectral estimator,
- full SVD,
- RSVD.

Rate:
- residual hum,
- speech damage,
- low-frequency naturalness,
- musical noise,
- pumping/flutter,
- overall cleanliness.

## Android device tests

Test at minimum:
- one Pixel/reference device,
- one Samsung device,
- one lower-cost Android device,
- analog wired route where available,
- two USB-C audio dongles/headsets.

Measure:
- frames per burst,
- callback timing,
- underruns,
- input/output latency,
- jitter,
- CPU and battery,
- route changes,
- background/foreground transitions.

## Acoustic cancellation gate

Do not enable human-worn tests until:
- dummy-head or coupler test passes,
- limiter and emergency mute are verified,
- output remains bounded during route changes,
- calibration expiration works,
- failure injection tests pass.
