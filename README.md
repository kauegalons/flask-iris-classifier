# Flask Iris Classifier

A Flask web application that lets you train and compare scikit-learn classifiers
on the classic Iris dataset directly from the browser. You pick an algorithm,
tune its main hyperparameters, and the app returns the evaluation metrics along
with a rendered confusion matrix.

Built as a final project for a Machine Learning course.

## What it does

1. You select a classifier and fill in the hyperparameters the form reveals for it.
2. The app splits the Iris dataset into training and test sets using the
   `random_state` you provide.
3. It fits the model, predicts on the test set, and computes accuracy, precision,
   recall and F1-score, all as weighted averages.
4. It renders the confusion matrix with matplotlib, saves it to
   `static/displayimg.png`, and displays it next to the metrics.

## Supported classifiers

The form shows only the parameters relevant to the selected algorithm.

| Option | Estimator | Parameter 1 | Parameter 2 |
| --- | --- | --- | --- |
| `knn` | `KNeighborsClassifier` | `n_neighbors` (int) | not used |
| `dt` | `DecisionTreeClassifier` | `criterion` (`gini`, `entropy`, `log_loss`) | `max_depth` (int) |
| `mlp` | `MLPClassifier` | `hidden_layer_sizes` (int) | `activation` (`relu`, `identity`, `logistic`, `tanh`) |
| `rf` | `RandomForestClassifier` | `n_estimators` (int) | `max_depth` (int) |

`random_state` is required for every classifier and controls the train/test
split, so the same value reproduces the same results.

## Project structure

```text
flask-iris-classifier/
├── app1.py                  # Flask routes and form handling
├── ml.py                    # dataset loading, training, evaluation, plotting
├── templates/
│   └── index.html           # form, dynamic parameter fields, results
└── static/
    ├── style.css            # styling
    └── displayimg.png       # confusion matrix, overwritten on each run
```

## Requirements

- Python 3.10 or newer, since `ml.py` uses `match`/`case` statements
- Flask
- scikit-learn
- matplotlib
- numpy

## Setup

```bash
python -m venv .venv

# Linux / macOS
source .venv/bin/activate
# Windows
.venv\Scripts\activate

pip install flask scikit-learn matplotlib numpy
```

## Running

```bash
python app1.py
```

Then open <http://127.0.0.1:5000> in your browser.

The app currently starts with `debug=True`, which enables the interactive
Werkzeug debugger. That is convenient for local development but must never be
exposed on a public address, since the debugger allows arbitrary code execution.
Keep it bound to localhost or set `debug=False` before deploying anywhere.

## Example

Selecting `rf` with `n_estimators = 100`, `max_depth = 3` and
`random_state = 42` trains a random forest on 112 samples, evaluates it on the
remaining 38, and renders a 3x3 confusion matrix labelled with the Iris species
names alongside the four metrics.

## License

Academic work, shared for reference.
