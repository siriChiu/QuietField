# Android implementation design

## Supported MVP configuration

- Android 12 or newer.
- Wired analog headset or USB-C digital headset.
- Microphone access granted while a foreground session is active.
- 48 kHz preferred.
- Low-latency and exclusive modes requested where supported, never assumed.

## Technology stack

- Kotlin.
- Jetpack Compose.
- Coroutines for non-real-time orchestration.
- Android foreground service for active analysis sessions.
- C++20 NDK library.
- Oboe using AAudio where available.
- CMake for native targets.

## Audio stream strategy

Use full-duplex input/output only when needed. Analysis-only mode may use input without generated output. Later cancellation mode requires synchronized streams and timestamp diagnostics.

Requested properties:
- performance mode low latency,
- sharing mode exclusive when available, fallback to shared,
- callback-based stream,
- native sample rate and frames-per-burst,
- float format where supported.

## Device capability report

Record:
- route type,
- input/output device IDs,
- sample rates,
- frames per burst,
- buffer sizes,
- estimated input/output/round-trip latency,
- jitter,
- underruns/xruns,
- microphone and output channel counts.

## Route policy

### Allowed for analysis
- Built-in microphone.
- Wired headset microphone.
- USB microphone/headset.
- Bluetooth microphone only for analysis experiments, not cancellation claims.

### Allowed for experimental cancellation
- Wired analog or USB-C route only.
- Stable sample rate.
- Successful calibration.
- Measured jitter below configured threshold.

## Lifecycle

- Session runs only with explicit user action.
- Foreground notification displays microphone use.
- Screen-off operation must retain session state safely.
- Route change immediately mutes generated output and returns to capability check.
- Audio focus loss mutes generated output.

## Permissions and privacy

- Request `RECORD_AUDIO` only when starting analysis.
- Explain that raw audio remains on device.
- Export is explicit and user-selected.
- Do not request unrelated storage access; use scoped storage/document picker.

## JNI boundary

JNI calls occur during setup/control only. The Java/Kotlin layer does not participate in each audio callback. Native handles own audio streams and DSP state.

## UI screens

### Home
- Start analysis.
- Current route and suitability.
- Clear non-ANC claim.

### Live analysis
- Dominant persistent bands.
- Estimated rank.
- Stability/confidence.
- CPU load and underruns.
- No raw speech transcription or semantic classification.

### Laboratory
- Import WAV.
- Run baselines and RSVD.
- Listen to original, estimated noise, and residual.
- Export report.

### Experimental cancellation
- Hidden/disabled until capability and safety gates pass.
- Calibration status.
- Conservative strength control.
- Persistent emergency mute button.
