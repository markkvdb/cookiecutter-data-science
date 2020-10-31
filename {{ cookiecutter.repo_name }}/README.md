# {{cookiecutter.project_name}}

## Description

{{cookiecutter.description}}

## Installation

```sh
# Clone this repository
gh clone <repo>

# Enter project directory
cd <repo_name>

# Install dependencies
pipenv install
```

## Usage

The project is divided into several steps. Every step has a corresponding `make` command. To get an overview of the options run

```sh
make help
```

To configure what functions or steps should be taken for each step I refer to the `Makefile` which contains the scripts and dependencies called for each command.

### Data Processing

First, the data is downloaded an processed into a form that is useable for the project. The idea is to transform and load external data (found in `data/external`) and raw immutable data (in `data/raw`). This can be done by running the following command.

```sh
make data
```

After running this script, the `data/processed` folder will be populated.

### Feature Engineering

Transform the processed data by extracting and building useful features and save them to the `data/interim` folder by running

```sh
make build_features
```

### Models

The final data required after the feature engineering step is used to train our models and use them to predict.

#### Training

Training all models can be done by calling

```sh
make train_models
```

This will populate the `models` folder with model objects of some form, e.g., tensorflow, pytorch or sklearn models.

Note that this script will run all models.

#### Predicting

TODO

### Reporting

Create figures, documents and other reporting output by running

```sh
make reports
```

This command will populate the `reports` folder.
