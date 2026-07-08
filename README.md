# LiveMatch Edge

In-play football outcome prediction and betting-strategy evaluation for IE 582.

LiveMatch Edge analyzes live match statistics and betting-market odds to predict match outcomes during play. The project compares XGBoost, multinomial logistic regression, and random forest models, then evaluates whether confidence-based live betting rules can produce useful decision signals.

> Course project repository for IE 582. This project is for academic analysis only and is not betting advice.

## Project Links

- [Project homepage](https://fbaakyildiz.github.io/IE582-LiveMatch-Edge/)
- [Final report](reports/final-report.html)
- [Rendered analysis notebook](reports/analysis-notebook.html)
- [Source notebook](notebooks/live_match_edge_workflow.ipynb)
- [Design document](docs/DESIGN.md)

## Problem Statement

Football match outcomes evolve minute by minute as goals, cards, shots, possession, and market odds change. The goal of this project is to estimate the final match result from in-play data and determine whether model confidence can be translated into a disciplined live betting decision rule.

The prediction target is the final match result:

- `1`: home win
- `X`: draw
- `2`: away win

## Method Summary

The workflow performs the following steps:

1. Load and time-order match-level event/statistics data.
2. Forward-fill live match features within fixtures.
3. Normalize bookmaker odds into implied probabilities.
4. Normalize percentage and continuous home/away statistics.
5. Split matches chronologically into train and test sets.
6. Train and tune three model families:
   - XGBoost classifier
   - Multinomial logistic regression
   - Random forest classifier
7. Evaluate predictive accuracy and confusion matrices.
8. Apply a live betting strategy using confidence thresholds and minute cutoffs.
9. Compare model decisions by correctness and cumulative return.

## Repository Structure

```text
.
├── README.md
├── index.md
├── _config.yml
├── requirements.txt
├── docs/
│   ├── DESIGN.md
│   └── archive-note.md
├── notebooks/
│   └── live_match_edge_workflow.ipynb
└── reports/
    ├── analysis-notebook.html
    └── final-report.html
```

## Reproducing the Notebook

Create an environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Open the notebook:

```bash
jupyter notebook notebooks/live_match_edge_workflow.ipynb
```

The notebook expects the original match dataset used in the course project. The raw data file is not committed to this repository.

## Design

[docs/DESIGN.md](docs/DESIGN.md) documents the data pipeline, model training flow, betting-strategy logic, repository layout, reproducibility constraints, and future improvements.

## Academic Disclaimer

This repository is an academic term project. Results are not intended for real-money wagering, production deployment, or financial decision-making.
