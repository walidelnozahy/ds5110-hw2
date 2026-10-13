# DS5110 Homework 2: tidy, reshape, join

This repo turns three raw files (quarterly revenue, orders, shipments) into one tidy, analysis-ready table.

- `hw2.ipynb`: the full analysis, Steps 0–5 in order, runnable top to bottom
- `WRITEUP.md`: answers to every question, in the same order
- `data/`: the three raw CSVs, unchanged
- `figures/`: the Step 0 raw-data charts and the Step 5 before/after chart
- `output/orders_analysis_ready.csv`: the final table (one row per order)

## Run it

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace hw2.ipynb
```

Or open `hw2.ipynb` in VS Code or Jupyter and choose Run All. Run it from the repo root, since paths are relative to it.

## Use of AI

I used Claude (Anthropic) to help scaffold the notebook structure, draft the plotting code, and draft a first version of the write-up. I checked every step against the data, reviewed and edited the code and the write-up line by line, and can explain each decision.
