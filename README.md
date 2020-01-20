[![CircleCI](https://circleci.com/gh/MaxBoykoII/devops-ml-microservice-kubernetes.svg?style=svg)](https://circleci.com/gh/MaxBoykoII/devops-ml-microservice-kubernetes)

## Project Overview

This project illustrates how to operationalize a Python flask app—in a provided file, `app.py`—that serves out predictions (inference) about housing prices through API calls. This project could be extended to any pre-trained machine learning model, such as those for image recognition and data labeling.

The model used in this project is a pre-trained, `sklearn` model that predicts housing prices in Boston according to several features, such as average rooms in a home and data about highway access, teacher-to-pupil ratios, and so on. You can read more about the data, which was initially taken from Kaggle, on [the data source site](https://www.kaggle.com/c/boston-housing).

The project includes scripts for creating a docker image of the flask app, uploading this image, and for running the image through Kubernetes.

Circleci has been integrated into the project and is used for automatically linting project files.

## Setup the Environment

- Create a virtualenv and activate it
- Run `make install` to install the necessary dependencies

### Running `app.py`

1. Standalone:  `python app.py`
2. Run in Docker:  `./run_docker.sh`
3. Run in Kubernetes:  `./run_kubernetes.sh`

## Making a prediction

Assuming `app.py` is running in Docker or Kubernetes, run `./make_prediction.sh`.

# Key Project Files

+ `.circleci/config.yml` contains the configuration for circleci, which automatically lints python files and the Dockerfile whenever new code is pushed to the repo.
+ `model_data/boston_housing_prediction.joblib` contains the pre-trained model.
+ `model_data/housing.csv` contains the data used to train the model.
+ `output_txt_files/docker_out.txt` shows the output of a sample prediction from the docker image  when `run_docker.sh` is running and `make_prediction.sh` is invoked.
+ `output_txt_files/kubernetes_out.txt` shows the output from Kubernetes when `make_prediciton.sh` is invoked and the Kubernetes deployment is running.
+ `app.py` contains the flask app, which itself has a POST route predicting housing prices and a default GET route that serves up a minimal home page. The POST also includes logging to assist in debugging the model, if necessary.
+ `Dockerfile` includes the steps for containerizing the flask app.
+ `make_prediction.sh` is a script for making a sample prediction. It assumes that the flask app is hosted on `http://localhost:8000`. 
+ `Makefile` includes commands for spinning up a virtual python environment (needed for local development, if, for example, it became necessary to extend the model or the  flask app), installing the python dependencies, and testing changes to python files and the Dockerfile.
+ `requirements.txt` lists the project dependencies.
+ `run_docker.sh` is a script for building and running the project image.
+ `run_kubernetes.sh` is a script for deploying the project image with Kubernetes. (Assumes that Kubernetes is running)
+ `upload_docker.sh` is a script for uploading the project image to maxboyko/devops-ml-microservice-kubernetes:v1

