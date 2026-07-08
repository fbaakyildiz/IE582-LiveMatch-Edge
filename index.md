# LiveMatch Edge

[Back to Portfolio](https://fbaakyildiz.github.io/)

In-play football outcome prediction and betting-strategy evaluation.

LiveMatch Edge is an IE 582 term project that uses live match statistics and bookmaker odds to predict final football match outcomes during play. The project compares XGBoost, multinomial logistic regression, and random forest models, then evaluates a confidence-based live betting strategy.

## Project Materials

| Resource | Link |
|---|---|
| Final report | [Open report](reports/final-report.html) |
| Rendered analysis notebook | [Open notebook HTML](reports/analysis-notebook.html) |
| Source notebook | [View notebook](notebooks/live_match_edge_workflow.ipynb) |
| Design document | [Open design document](docs/DESIGN.md) |
| GitHub repository | [fbaakyildiz/IE582-LiveMatch-Edge](https://github.com/fbaakyildiz/IE582-LiveMatch-Edge) |

## What The Project Does

- Converts betting odds into normalized implied probabilities.
- Builds live, minute-level feature sets from match statistics.
- Splits data chronologically to reduce look-ahead bias.
- Trains and compares XGBoost, logistic regression, and random forest classifiers.
- Evaluates prediction quality with accuracy, classification reports, and confusion matrices.
- Simulates live betting decisions using confidence thresholds and cutoff minutes.

## Academic Disclaimer

This project is for academic analysis only. It is not betting advice and should not be used for real-money wagering.
