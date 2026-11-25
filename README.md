# Offline Document Intelligence Engine

Extracts structured data (invoice number, date, vendor, subtotal, tax, total) from
scanned invoices and receipts — **100% on-device, no API calls, no cloud**. Built
for finance teams that cannot send financial documents to cloud LLMs for compliance.

## Results

Field-level F1 on 50 synthetic invoices with known ground truth.

| Backend | Invoice # | Date | Vendor | Subtotal | Tax | Total | Overall F1 |
|---|---|---|---|---|---|---|---|
| rules | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` |
| hybrid | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` | `<TBD>` |

Fill in from `python scripts/run_eval.py`. Delete the `llm`/`hybrid` row if Ollama
isn't installed — do not invent numbers.

## What it does

1. **OCR** — Tesseract (offline) on PNG/JPG, optional PDF via pdf2image
2. **Extract** — rule-based regex/heuristics (default), local LLM via Ollama (optional), or hybrid
3. **Validate** — checks that subtotal + tax ≈ total, and that required fields exist
4. **Export** — CSV + JSON with per-row validation flags

The validation step is the point: it turns silent extraction errors into visible
data-quality flags, which is what actually matters in production pipelines.

## Run it (fully offline)

```bash
sudo apt install tesseract-ocr          # system dep (one-time)
pip install -r requirements.txt

python scripts/generate_samples.py      # 50 synthetic invoices + ground truth
python -m src.cli extract --backend rules   # → out/results_rules.csv
python scripts/run_eval.py              # → out/report.md
pytest -q