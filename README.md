# Flask Iris Classifier

  A small Flask web application that lets you train and compare scikit-learn
  classifiers on the classic Iris dataset directly from the browser. You pick an
  algorithm, tune its main hyperparameters, and the app returns the evaluation
  metrics along with a rendered confusion matrix.

  Built as a project for a Machine Learning class.

  ## What it does

  1. You select a classifier and fill in the hyperparameters the form reveals for it.
  2. The app splits the Iris dataset into training and test sets using the
     `random_state` you provide.
  3. It fits the model, predicts on the test set, and computes accuracy, precision,
     recall and F1-score (all weighted averages).
  4. It renders the confusion matrix with matplotlib, saves it to
     `static/displayimg.png`, and displays it next to the metrics.

  ## Supported classifiers

  The form shows only the parameters relevant to the selected algorithm.

  | Option | Estimator | Parameter 1 | Parameter 2 |
  |---|---|---|---|
  | `knn` | `KNeighborsClassifier` | `n_neighbors` (int) | not used |
  | `dt` | `DecisionTreeClassifier` | `criterion` (`gini`, `entropy`, `log_loss`) | `max_depth` (int) |
  | `mlp` | `MLPClassifier` | `hidden_layer_sizes` (int) | `activation` (`relu`, `identity`, `logistic`, `tanh`) |
  | `rf` | `RandomForestClassifier` | `n_estimators` (int) | `max_depth` (int) |

  `random_state` is required for every classifier and controls the train/test split,
  so the same value reproduces the same results.

  ## Project structure

  app1.py              Flask routes and form handling
  ml.py                dataset loading, training, evaluation and plotting
  templates/index.html form, dynamic parameter fields and results
  static/style.css     styling
  static/displayimg.png confusion matrix, overwritten on each run

  ## Requirements

  - Python 3.10 or newer (`ml.py` uses `match`/`case` statements)
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

  Running

  python app1.py

  Then open http://127.0.0.1:5000 in your browser.

  The app currently starts with debug=True, which enables the interactive
  Werkzeug debugger. That is convenient for local development but must never be
  exposed on a public address, since the debugger allows arbitrary code execution.
  Keep it bound to localhost or set debug=False before deploying anywhere.

  Known limitations

  Documented for clarity, as this is a course project rather than a production app:

  - The confusion matrix is written to a single fixed path (static/displayimg.png),

    so concurrent users overwrite each other's results. Browser caching may also
    show a stale image until a hard refresh.

  - matplotlib figures are never closed, so each request leaks a figure into memory.

    Long sessions will grow the process footprint.

  - Form input is not validated server-side. The dropdown's initial Selecione...

    option keeps the submit button disabled in the browser, but a direct POST with
    an unexpected classifier value will raise an error instead of returning a
    friendly message.

  - Numeric fields are converted with int() without error handling, so

    non-numeric input raises an unhandled exception.

  - Only the Iris dataset is supported. There is no option to upload or choose

    other data.

  - __pycache__ is committed to the repository and there is no .gitignore.
  - The UI labels and the source comments are in Portuguese.

  License

  Academic work, shared for reference.
