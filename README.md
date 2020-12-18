# Teeport Test

This serves as a guide to deploy the Teeport platform and test the basic features of Teeport.

## Prerequisites

To follow this guide, you'll need:

- [conda](https://www.anaconda.com/) 4.8+

And one of the following two:

- [docker](https://www.docker.com/) 19.03+
- [Node.js](https://nodejs.org/en/) 12+

If you choose the docker path and you'd like to test the optional frontend part of Teeport, then you'll need docker 19.03 or higher (we'll use the new BuildKit mode provided since that version), or else you can use any relatively new version of docker.

If you encounter any issues while following this guide, please open an issue/discussion or [shoot me an email](mailto:zhezhang@slac.stanford.edu).

## Build and run the backend service

Please follow the instructions in the [teeport-backend](https://github.com/SPEAR3-ML/teeport-backend) project. You can either go with the [Run the service directly](https://github.com/SPEAR3-ML/teeport-backend#run-the-service-directly) section or the [Run the service in docker](https://github.com/SPEAR3-ML/teeport-backend#run-the-service-in-docker) section, but not both.

## Install the Teeport client for python

Assume that you have some evaluate/optimize functions written in python, in order to enable them to talk to Teeport, you need to install the Teeport client for python.

### [Optional] Setup a new environment

In case you don't want to mess with your current python environment, creating a new virtual environment would be a good option.

```bash
conda create -n teeport python=3.7
conda activate teeport
```

### Clone the repo

```bash
git clone https://github.com/SPEAR3-ML/teeport-client-python.git
```

### Install the Teeport client in editable mode

You can check the dependences of the Teeport client by viewing `setup.py`, you'll find that the dependencies are: `numpy`, `websockets` and `anytree`. The first two are essential for Teeport to work, the last one is only needed for the future possible features.

Now install the client:

```bash
cd teeport-client-python
pip install -e .
```

## Test Teeport w/o GUI

**Note: no need to do this step in the production environment!** (because Jupyter Lab depends on too many packages, while we'd better keep the production env as clean and simple as possible)

Now we are ready to test Teeport. I recommend to perform test in an interactive environment so you get the idea of what is going on. Below we'll do the tests in **Jupyter Lab**.

### Install the Jupyter Lab module

```bash
conda install -c conda-forge jupyterlab
```

### Clone the Teeport test notebooks/scripts

Actually you might have already done this, since this document itself is the readme of the repo to be cloned lol:

```bash
git clone https://github.com/SPEAR3-ML/teeport-test.git
```

### Run the test notebooks in Jupyter Lab

```bash
cd teeport-test
jupyter lab
```

In the newly opened tab in your browser, double click the `basic-tests.ipynb` file in the file panel to open it in the Lab. Then just follow the instructions in the notebook to get the basic ideas of how to use Teeport.

## [Optional] Build and run the frontend service

Please follow the instructions in the [teeport-frontend](https://github.com/SPEAR3-ML/teeport-frontend) project. You can either go with the [Run the service directly](https://github.com/SPEAR3-ML/teeport-frontend#run-the-service-directly) section or the [Run the service in docker](https://github.com/SPEAR3-ML/teeport-frontend#run-the-service-in-docker) section, but not both.

Now the Teeport frontend should be accessible at port 3000. To verify that, open your favorite browser and go to [http://localhost:3000/tasks](http://localhost:3000/tasks). If you're going though this document step by step, you should be able to see the tasks created when you played with the `basic-tests.ipynb` in the last section.

Hover your mouse on the status bar (where shows the time created) of each task card will give you more options to interact with the task. Say, click on the Enter button to access the monitors of that task, you'll see the evaluation history and stuff. Try to relate the plots to what you did in the notebook to get more feelings about the mechanism of Teeport.

## [Optional] Test Teeport w/ GUI

### Run the GUI test notebooks in Jupyter Lab

In the Jupyter Lab tab in your browser, double click the `gui-tests.ipynb` file in the file panel to open it in the Lab. Then just follow the instructions in the notebook to test the GUI functionalities of Teeport.
