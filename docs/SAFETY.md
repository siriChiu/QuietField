# Safety and failure handling

## Principle

Incorrect anti-noise can amplify sound. The system must fail silent, not fail loud.

## Mandatory controls

- Generated output disabled by default.
- Hard peak limiter and RMS/energy limiter.
- Configurable maximum cancellation gain.
- DC blocker.
- Band-limited generated output.
- Model confidence gate.
- Model age timeout.
- Calibration validity check.
- Route-type check.
- Underrun/xrun watchdog.
- Emergency mute available from UI and hardware-volume response.
- Immediate mute on route change, focus loss, stream error, or app pause.

## Initial limits

Exact values must be validated experimentally, but initial research defaults should be conservative:
- cancellation band no wider than 30–250 Hz,
- partial inversion gain far below unity,
- gradual fade-in/fade-out,
- no generated output when clipping is detected,
- no cancellation while calibration tones are playing unless explicitly designed.

## Safety states

```text
SafeMuted -> CalibratedMuted -> FadingIn -> Active
Active -> FadingOut -> SafeMuted
Any state -> FaultMuted
```

No direct transition from startup to `Active`.

## Human testing

- Start on acoustic fixture/coupler.
- Use low output levels.
- Require informed tester action.
- Stop immediately on discomfort, pressure sensation, oscillation, clicks, or unexpected amplification.
- Preserve alarms and situational awareness; the project does not intentionally cancel safety-critical signals.
