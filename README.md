# Structural Risk Scorer — Evidence Pack

This repository is **evidence-only**.

It contains aggregated audit artifacts produced by a proprietary
structural-risk engine (ISCE). The runnable engine and calibration
are not included.

## What is included
See `proofpack/`:
- summary.csv
- report.json
- actions.png
- regimes.png

These artifacts show how real reasoning traces were classified into:
EXECUTE / HOLD / BLOCK and regimes T1–T5.

## Offline pilot (how to validate)
See `pilot/README.md`.

For an offline pilot, share only:
- pilot_out/summary.csv
- pilot_out/report.json
- pilot_out/actions.png
- pilot_out/regimes.png

No raw traces required.

## Why the engine is not public
Any runnable artifact that allows human+LLM iteration becomes
a gradient exploitable by incumbents.

This repository publishes **results**, not the machine that produced them.
