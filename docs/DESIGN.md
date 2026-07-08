# LiveMatch Edge Design Document

Developer-level design notes for the IE 582 football analytics term project.

## 1. Project Identity

**Project name:** LiveMatch Edge

**Recommended GitHub repository name:** `IE582-LiveMatch-Edge` or `live-match-edge`

The project studies in-play football match prediction. It combines live match statistics, betting-market odds, and machine learning classifiers to predict final match outcomes and evaluate confidence-based betting decisions.

## 2. Scope

The project answers two technical questions:

1. Can live football match statistics and betting odds predict final outcomes during the match?
2. Can model confidence be used as a disciplined trigger for live betting decisions?

The project is academic. It is not designed for real-money betting, automated trading, or production deployment.

## 3. Repository Layout

```text
.
├── README.md                         # GitHub landing page
├── index.md                          # GitHub Pages homepage
├── _config.yml                       # GitHub Pages configuration
├── requirements.txt                  # Python analysis dependencies
├── docs/
│   ├── DESIGN.md                     # Developer design document
│   └── archive-note.md               # Original placeholder content
├── notebooks/
│   └── live_match_edge_workflow.ipynb # Source analysis workflow
└── reports/
    ├── final-report.html             # Final rendered report
    └── analysis-notebook.html        # Rendered code notebook
```

## 4. Data Flow

```text
Raw match data
    ↓
Time ordering by fixture, half, minute, second
    ↓
Missing-value forward fill for live features
    ↓
Second-half minute adjustment
    ↓
Odds conversion to implied probabilities
    ↓
Percentage and home/away feature normalization
    ↓
Chronological train/test split
    ↓
Model training and tuning
    ↓
Prediction evaluation
    ↓
Live betting strategy simulation
```

## 5. Data Assumptions

The notebook expects a match-level CSV with fields such as:

- `fixture_id`
- `halftime`
- `minute`
- `second`
- bookmaker odds columns: `1`, `X`, `2`
- final `result`
- match statistics split by home and away teams
- `match_start_datetime`

The original dataset is not committed to the repository. To fully reproduce the notebook, place the source CSV in the expected notebook working directory or update the notebook path.

## 6. Feature Engineering

### Time Ordering

Rows are sorted by fixture and match clock to preserve the live sequence of information.

### Missing Values

Live match statistics are forward-filled after ordering. Remaining missing values are filled with zero.

### Match Clock Normalization

Rows from the second half are shifted by adding 45 minutes to represent full-match elapsed time.

### Odds Normalization

Bookmaker odds are converted into implied probabilities:

```text
P(outcome) = 1 / decimal_odds
```

The probabilities are then normalized so that home win, draw, and away win probabilities sum to one.

### Continuous Feature Normalization

Home and away statistics are normalized relative to their paired totals where possible. Percentage features are converted from percentage points to decimals.

## 7. Modeling

The notebook compares three supervised classifiers.

| Model | Purpose |
|---|---|
| XGBoost | Nonlinear boosted-tree baseline with hyperparameter tuning |
| Multinomial logistic regression | Interpretable generalized linear model baseline |
| Random forest | Ensemble tree model for nonlinear feature interactions |

The target is the final match result:

```text
1 = home win
X = draw
2 = away win
```

## 8. Training Strategy

The data is split chronologically using match start time. Matches before the split date are used for training, and later matches are used for testing.

This is preferred over random splitting because football match data has time-dependent structure and market behavior may shift over time.

## 9. Evaluation

Model evaluation includes:

- test-set accuracy
- classification report
- confusion matrix
- betting-strategy return simulation
- model decision comparison across fixtures

Accuracy measures classification quality. Return simulation evaluates whether correct predictions occur under useful odds and confidence conditions.

## 10. Live Betting Strategy

The strategy iterates through each fixture minute by minute:

1. Build the current feature vector from information available at that minute.
2. Predict class probabilities with the trained model.
3. Select the highest-probability outcome.
4. Place a simulated bet only when confidence exceeds a threshold.
5. Stop considering bets after a cutoff minute.
6. Evaluate outcome against the final match result and odds at the decision minute.

Important strategy parameters:

- initial confidence threshold
- confidence step as match time progresses
- cutoff minute
- model probability estimates

## 11. Generated Artifacts

The repository includes rendered HTML artifacts for easier review:

- `reports/final-report.html`
- `reports/analysis-notebook.html`

These files make the project readable on GitHub Pages without requiring users to execute the notebook.

## 12. Reproducibility

To reproduce the notebook:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/live_match_edge_workflow.ipynb
```

Known reproducibility constraints:

- the raw dataset is not committed
- notebook paths may need to be updated depending on where the CSV is stored
- XGBoost and scikit-learn versions can affect model results
- randomized model search should use fixed seeds for exact repeatability

## 13. Security and Ethics

This repository does not require secrets or API keys.

Ethical constraints:

- Do not present model outputs as guaranteed betting advice.
- Do not use academic results for real-money wagering without rigorous validation.
- Avoid overclaiming performance from one test split.
- Treat bookmaker odds as market signals, not objective probabilities.

## 14. Current Limitations

- Raw input data is not included.
- Feature engineering is embedded in a notebook rather than a reusable Python package.
- Model training and strategy simulation are not covered by automated tests.
- Betting returns are simplified and do not include transaction costs, liquidity, bookmaker limits, or odds movement after decision time.
- Confidence thresholds are heuristic and should be validated more systematically.

## 15. Future Engineering Improvements

Recommended next steps:

- Extract preprocessing into reusable Python modules.
- Add a config file for dataset paths and split dates.
- Add deterministic train/test scripts.
- Add unit tests for odds normalization and betting-return calculations.
- Store model metrics in machine-readable JSON.
- Add experiment tracking for model parameters and results.
- Add a lightweight dashboard for match-level decision inspection.

## 16. Rename Plan

Git repository renaming cannot be done through normal `git push`; it requires GitHub repository admin rights.

Recommended new repository name:

```text
IE582-LiveMatch-Edge
```

GitHub UI path:

```text
Repository -> Settings -> General -> Repository name -> Rename
```

After renaming, update the local remote:

```bash
git remote set-url origin https://github.com/fbaakyildiz/IE582-LiveMatch-Edge.git
```
