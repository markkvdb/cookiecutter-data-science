#################################################################################
# GLOBALS                                                                       #
#################################################################################

PROJECT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
BUCKET = {{ cookiecutter.s3_bucket }}
PROFILE = {{ cookiecutter.aws_profile }}
PROJECT_NAME = {{ cookiecutter.repo_name }}

PYTHON_INTERPRETER = pipenv run python

DATA_DIR := $(realpath ./data)
DATA_DIR_EXTERNAL = $(DATA_DIR)/external
DATA_DIR_INTERIM = $(DATA_DIR)/interim
DATA_DIR_PROCESSED = $(DATA_DIR)/processed
DATA_DIR_RAW = $(DATA_DIR)/raw

DATA_EXTERNAL_FILES := $(wildcard $(DATA_DIR_EXTERNAL)/*)
DATA_RAW_FILES := $(wildcard $(DATA_DIR_RAW)/*)
DATA_PROCESSED_FILES := $(wildcard $(DATA_DIR_PROCESSED)/*)
DATA_INTERIM_FILES := $(wildcard $(DATA_DIR_INTERIM)/*)

SRC_DIR := $(realpath ./src)
SRC_DIR_DATA = $(SRC_DIR)/data
SRC_DIR_FEATURES = $(SRC_DIR)/features
SRC_DIR_MODELS = $(SRC_DIR)/models
SRC_DIR_REPORTS = $(SRC_DIR)/reports

SRC_DATA_FILES := $(wildcard $(SRC_DIR_DATA)/*.py)
SRC_FEATURES_FILES := $(wildcard $(SRC_DIR_FEATURES)/*.py)
SRC_MODELS_FILES := $(wildcard $(SRC_DIR_MODELS)/*.py)

MODEL_DIR := $(realpath ./models)
MODEL_FILES := $(wildcard $(MODEL_DIR)/*)

REPORT_DIR := $(realpath ./reports)
REPORT_FILES := $(wildcard $(REPORT_DIR)/*)
FIGURE_FILES := $(wildcard $(REPORT_DIR)/figures/*)

#################################################################################
# COMMANDS (USER-DEFINED)                                                       #
#################################################################################

.PHONY: data features train_models predict_models report clean_all \
 	clean_python clean_output install sync_data_to_s3 sync_data_from_s3 set_pre_hooks

## Preprocess raw data
data: $(SRC_DATA_FILES) $(DATA_EXTERNAL_FILES) $(DATA_RAW_FILES)
	$(PYTHON_INTERPRETER) $(SRC_DIR_DATA)/make_dataset.py

## Build features from preprocessed data
features: data $(SRC_FEATURES_FILES)
	$(PYTHON_INTERPRETER) $(SRC_DIR_FEATURES)/build_features.py

## Train models
train_models: features $(SRC_DIR_MODELS)/train_model.py
	$(PYTHON_INTERPRETER) $(SRC_DIR_MODELS)/train_model.py

## Predict models
predict_models: train_models $(SRC_DIR_MODELS)/predict_model.py
	$(PYTHON_INTERPRETER) $(SRC_DIR_MODELS)/predict_model.py

## Create all reporting documents (TODO)
reports:

#################################################################################
# COMMANDS (DO NOT TOUCH)                                                       #
#################################################################################

## Clean all files
clean_all: clean_python clean_output

## Delete all compiled Python files
clean_python:
	find . -type f -name "*.py[co]" -delete
	find . -type d -name "__pycache__" -delete

## Clean processed and interim data
clean_output:
	rm -f $(DATA_PROCESSED_FILES)
	rm -f $(DATA_INTERIM_FILES)
	rm -f $(MODEL_FILES)
	rm -f $(REPORT_FILES)
	rm -f $(FIGURE_FILES)

## Upload Data to S3
sync_data_to_s3:
ifeq (default,$(PROFILE))
	aws s3 sync data/ s3://$(BUCKET)/data/
else
	aws s3 sync data/ s3://$(BUCKET)/data/ --profile $(PROFILE)
endif

## Download Data from S3
sync_data_from_s3:
ifeq (default,$(PROFILE))
	aws s3 sync s3://$(BUCKET)/data/ data/
else
	aws s3 sync s3://$(BUCKET)/data/ data/ --profile $(PROFILE)
endif

## Set up python interpreter environment
install:
	pipenv install

## Install prehooks
set_pre_hooks:
	pipenv run pre-commit install -t pre-commit
	pipenv run pre-commit install -t pre-push

#################################################################################
# PROJECT RULES                                                                 #
#################################################################################



#################################################################################
# Self Documenting Commands                                                     #
#################################################################################

.DEFAULT_GOAL := help

# Inspired by <http://marmelab.com/blog/2016/02/29/auto-documented-makefile.html>
# sed script explained:
# /^##/:
# 	* save line in hold space
# 	* purge line
# 	* Loop:
# 		* append newline + line to hold space
# 		* go to next line
# 		* if line starts with doc comment, strip comment character off and loop
# 	* remove target prerequisites
# 	* append hold space (+ newline) to line
# 	* replace newline plus comments by `---`
# 	* print line
# Separate expressions are necessary because labels cannot be delimited by
# semicolon; see <http://stackoverflow.com/a/11799865/1968>
.PHONY: help
help:
	@echo "$$(tput bold)Available rules:$$(tput sgr0)"
	@echo
	@sed -n -e "/^## / { \
		h; \
		s/.*//; \
		:doc" \
		-e "H; \
		n; \
		s/^## //; \
		t doc" \
		-e "s/:.*//; \
		G; \
		s/\\n## /---/; \
		s/\\n/ /g; \
		p; \
	}" ${MAKEFILE_LIST} \
	| LC_ALL='C' sort --ignore-case \
	| awk -F '---' \
		-v ncol=$$(tput cols) \
		-v indent=19 \
		-v col_on="$$(tput setaf 6)" \
		-v col_off="$$(tput sgr0)" \
	'{ \
		printf "%s%*s%s ", col_on, -indent, $$1, col_off; \
		n = split($$2, words, " "); \
		line_length = ncol - indent; \
		for (i = 1; i <= n; i++) { \
			line_length -= length(words[i]) + 1; \
			if (line_length <= 0) { \
				line_length = ncol - indent - length(words[i]) - 1; \
				printf "\n%*s ", -indent, " "; \
			} \
			printf "%s ", words[i]; \
		} \
		printf "\n"; \
	}' \
	| more $(shell test $(shell uname) = Darwin && echo '--no-init --raw-control-chars')
