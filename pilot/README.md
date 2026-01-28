# Pilot Kit (offline) — audit a directory of trace JSONs

This pilot runs **locally** and produces a small set of **shareable artifacts**
without requiring you to share raw traces.

---

## Input

A directory containing trace exports as JSON in the scorer input format:

- Either LangSmith exports you converted, or any JSON that looks like:

```json
{
  "items": [{
    "id": "RUN_ID",
    "domain": "ANY",
    "text": ["step 1", "step 2", "step 3", "step 4"]
  }]
}
```

## Run (one command)

From repo root:

```bash
PYTHONPATH=. python3 pilot/audit_dir.py \
  --in_dir <DIR_WITH_JSON> \
  --out_dir pilot_out \
  --pattern "*_llm.json,*.json" \
  --limit 50
```
## Quick demo (no LangSmith required)

If you don’t have traces yet, create a tiny local directory:

```bash
mkdir -p sample_traces

cat > sample_traces/run_ok.json <<'JSON'
{"items":[{"id":"RUN_OK","domain":"TEST","text":["step 1","step 2","step 3","step 4"]}]}
JSON

cat > sample_traces/run_bad.json <<'JSON'
{"items":[{"id":"RUN_BAD","domain":"TEST","text":["loop","loop","loop","loop","loop","loop","loop","loop"]}]}
JSON
```
## Run the audit:

```bash
PYTHONPATH=. python3 pilot/audit_dir.py \
  --in_dir sample_traces \
  --out_dir pilot_out \
  --pattern "*.json" \
  --limit 50
```

## Offline-first note (important)

This pilot uses a SentenceTransformer embedder by default.

If you want a fully offline run

You must have the model cached locally once.

Cache it once (online):
```bash
python3 - <<'PY'
from sentence_transformers import SentenceTransformer
SentenceTransformer("paraphrase-multilingual-mpnet-base-v2")
print("MODEL_CACHED_OK")
PY
```

Then you can force offline mode:
```bash
export HF_HUB_OFFLINE=1
export TRANSFORMERS_OFFLINE=1
export TOKENIZERS_PARALLELISM=false
```
Now the pilot command will run without network access.

## If HuggingFace is rate-limiting you

Either:
	•	cache once as above, then run offline, or
	•	set an HF token (optional) for higher rate limits:

```bash
export HF_TOKEN="<your_token>"
```

## Output

After running, pilot_out/ contains:
	•	summary.csv
	•	report.json
	•	actions.png
	•	regimes.png
	•	cases/ (per-run artifacts; local only)

⸻

What to share (offline pilot)

Share only these four files:
	•	pilot_out/summary.csv
	•	pilot_out/report.json
	•	pilot_out/actions.png
	•	pilot_out/regimes.png

Do not share raw traces.
The pilot is designed so you can share only the outputs.

