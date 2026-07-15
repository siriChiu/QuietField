# ADR 0002: RSVD is the noise-model estimator, not the real-time controller

## Status
Accepted.

## Decision
Use RSVD in the analysis worker to identify persistent low-rank noise components. Do not run RSVD in the callback or treat `-iSTFT(low-rank)` as a complete ANC controller.

## Rationale
RSVD can provide clean structural separation, while acoustic cancellation additionally requires prediction, delay compensation, path correction, and safety control.

## Consequences
The architecture has separate slow and fast paths. RSVD remains central but is evaluated against simpler baselines.
