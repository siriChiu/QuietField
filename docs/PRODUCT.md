# Product definition

## Problem

Many people need a quieter environment to study, work, read, or rest, but cannot afford premium ANC headphones. Ordinary passive headphones reduce some high-frequency noise but often leave persistent low-frequency hum and rumble.

## Vision

Provide a low-cost software path that reduces the most fatiguing persistent background noise using the Android phone and an ordinary wired or USB-C headset.

## User value

- Use existing hardware where possible.
- Operate fully offline.
- Focus on steady noise rather than making unrealistic full-ANC claims.
- Show whether the current environment and audio route are suitable.
- Degrade safely: when confidence is low, reduce gain or stop cancellation.

## Primary users

- Students in dormitories, libraries, cafés, or transport.
- Office workers near HVAC, fans, servers, or machinery.
- Remote workers with continuous household noise.
- Travelers on buses, trains, and aircraft, subject to route suitability.

## Product modes

### Analyze

Detect and visualize persistent low-frequency noise. This is the first production-quality feature.

### Preview

Offline or recorded-signal demonstration of estimated noise and residual signal.

### Experimental cancel

Opt-in, wired/USB-C-only acoustic anti-noise research mode. Hidden behind device validation and safety checks until proven.

## Success metrics

### DSP quality

- Persistent-noise estimation error.
- Residual attenuation in target bands.
- Signal distortion outside target bands.
- Artifact rate and subjective cleanliness.

### Runtime

- CPU load.
- Memory use.
- Battery drain.
- Analysis update latency.
- Audio callback underrun rate.

### Product safety

- Maximum generated output.
- Fail-safe mute response.
- False cancellation attempts.
- Route-change recovery.

## Claims policy

Allowed initial claim:

> Detects and models persistent low-frequency background noise. Experimental reduction is available only on supported wired routes.

Disallowed initial claims:

- “Turns any headset into premium ANC.”
- “Cancels all noise.”
- “Works equally over Bluetooth.”
- “Removes voices and sudden sounds.”
