# ADR 0003: Use complex STFT for the primary research path

## Status
Accepted.

## Decision
Use complex STFT matrices for RSVD experiments. Magnitude-only analysis may be retained as a diagnostic baseline.

## Rationale
Future coherent reconstruction and prediction require phase evolution, which magnitude-only decomposition discards.

## Consequences
Implementation and basis tracking are more complex, so full-SVD references and reconstruction tests are mandatory.
