# DSP design

## Objective

Estimate persistent low-frequency environmental noise more cleanly than simple filtering, then prepare a model suitable for future prediction and conservative anti-noise generation.

## Signal model

Let microphone input be:

\[
x[n] = n_p[n] + n_t[n] + s[n] + v[n]
\]

where:
- \(n_p\): persistent predictable background noise,
- \(n_t\): transient noise,
- \(s\): potentially meaningful sound such as speech or alarms,
- \(v\): sensor/electronic noise.

The MVP estimates only \(n_p\).

## Preprocessing

- DC removal.
- Optional high-pass at 15–20 Hz.
- Analysis low-pass or band selection up to 400 Hz.
- Level normalization for analysis only.
- Saturation detection.

Do not destructively filter the full-band source used for diagnostics; maintain a dedicated low-frequency analysis branch.

## Complex STFT

Default research configuration:
- sample rate: 48 kHz,
- FFT size: 512 or 1024,
- hop size: 64–256,
- Hann or square-root Hann window,
- overlap-add reconstruction tests required.

Form rolling matrix:

\[
X_t = [x_{t-T+1}, \ldots, x_t] \in \mathbb{C}^{F\times T}
\]

where each column is a complex STFT frame restricted to the analysis band.

## RSVD

Approximate:

\[
X \approx U_r \Sigma_r V_r^H
\]

Implementation requirements:
- fixed RNG seed in tests,
- configurable target rank \(r\), oversampling \(p\), and power iterations \(q\),
- full truncated-SVD reference for small matrices,
- benchmark approximation error and runtime,
- basis alignment between updates,
- singular-value smoothing,
- rank hysteresis.

## Why complex data

Magnitude-only SVD can identify energy structure but loses phase evolution. Complex STFT preserves information needed for coherent reconstruction and future predictive cancellation experiments.

## Component qualification

A singular component is a persistent-noise candidate only if it satisfies a weighted score:

\[
Q_i = w_E E_i + w_P P_i + w_L L_i + w_C C_i + w_S S_i - w_R R_i
\]

where:
- \(E_i\): energy contribution,
- \(P_i\): temporal persistence,
- \(L_i\): low-frequency concentration,
- \(C_i\): coherence across windows/channels,
- \(S_i\): subspace stability,
- \(R_i\): transient or semantic-risk penalty.

Use hysteresis for component admission/removal.

## Noise reconstruction

For accepted component set \(\mathcal{N}\):

\[
\hat N = \sum_{i\in\mathcal{N}} \sigma_i u_i v_i^H
\]

Residual:

\[
\hat S = X - \hat N
\]

Export both through iSTFT for listening and measurement.

## Baselines

Compare with:
- fixed high-pass/band-stop filters,
- adaptive notch bank,
- spectral subtraction or Wiener estimator,
- full truncated SVD,
- incremental/subspace tracking method,
- RSVD.

RSVD is retained only if it improves at least one meaningful dimension without unacceptable regressions:
- cleaner residual,
- better persistent-noise estimate,
- lower runtime/memory,
- better multi-source tracking,
- fewer artifacts.

## Prediction stage (later)

Predict low-dimensional component coefficients rather than the full waveform:

\[
a_i[t] = \sigma_i v_i[t]
\]

Candidate predictors:
- autoregressive model,
- PLL for tonal components,
- Kalman filter,
- NLMS predictor.

Forecast:

\[
\hat X_{t+h}=U_r\hat a[t+h]
\]

## Experimental anti-noise stage

Generated output is not simply `-iSTFT(N)`.

Use:

\[
y[n] = -\alpha C(z)\hat n[n+L]
\]

where:
- \(L\): estimated route delay,
- \(C(z)\): headphone/secondary-path correction,
- \(\alpha\): conservative gain,
- limiter and watchdog always follow.

Initial \(\alpha\) must be low and dynamically reduced when confidence or route stability declines.
