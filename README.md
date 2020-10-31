# Cookiecutter Data Science with Best Practices

This template uses [drivendata](http://drivendata.github.io/cookiecutter-data-science/)'s template as base but adds the best practices as introduced by [Sourcery.ai](https://sourcery.ai/blog/python-best-practices/).

## Quickstart

```sh
# Install pipx if pipenv and cookiecutter are not installed
python3 -m pip install pipx
python3 -m pipx ensurepath

# Install pipenv using pipx
pipx install pipenv

# Use cookiecutter to create project from this template
pipx run cookiecutter gh:markkvdb/cookiecutter-data-science

# Enter project directory
cd <repo_name>

# Initialise git repo
git init

# Install dependencies
make install

# Setup pre-commit and pre-push hooks
make set_pre_hooks
```

## Resulting Directory Structure

The directory structure of your new project looks like this:

```console
├── LICENSE
├── Makefile           <- Makefile with commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── Dockerfile         <- The dockerfile to set up this project in a encapsulated container.
│
├── Pipfile            <- Configuration file containing all information to build your package,
│                         e.g., it contains requirements, scripts and the pypi URL.
│
├── src                <- Source code for use in this project.
│   ├── __init__.py    <- Makes src a Python module
│   │
│   ├── data           <- Scripts to download or generate data
│   │   └── make_dataset.py
│   │
│   ├── features       <- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         <- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  <- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
│
└── setup.cfg          <- tox file with settings for running tox; see tox.readthedocs.io
```

## Tips & Comments

- This cookiecutter template is based on the [data-science template](https://github.com/drivendata/cookiecutter-data-science). Check the documentation for extensions and manual configurations.

- Advanced users can adapt the `Makefile` to their taste: add dependencies, add commands, etc. Furthermore, the `make` environment can be replaced by something more advanced like [Airflow](https://airflow.apache.org).
