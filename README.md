# Teeport Test

This serves as a guide to deploy the Teeport platform and test the basic features of Teeport.

## Prerequisites

Check if the **conda** and **docker** environments are available on the computer.

I use conda 4.8.3 and docker 1.12.0., I didn't test if the slightly old versions work, but I assume so since I didn't use any specific new features of conda and docker.

## Build and run the backend service

### Clone the repo

```bash
git clone https://github.com/SPEAR3-ML/teeport-backend.git
```

### Build the docker image

Go to the project directory:

```bash
cd teeport-backend
```

Create a file named `.env` with the content below:

```
NODE_ENV=production
PORT=8080
```

Now build the docker image:

```bash
docker build -t teeport/backend .
```

### Run the docker image

```bash
docker run -d -p 8080:8080 --name teeport-backend --restart always teeport/backend
```

Now the Teeport backend service should be running at port `8080`. You can verify that by running:

```bash
docker logs -f teeport-backend
```

And the print out should be something like:

```
================== Teeport Server v0.3.0 ==================
Service starting time : 2020-10-22 22:47:06
Service mode          : production
Service log level     : info
Service port          : 8080
```

Seeing this without an error, you can proceed to the next step.

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

Actually you might have already done this, since this document itself is the readme of the repo to be clone lol:

```bash
git clone https://github.com/SPEAR3-ML/teeport-test.git
```

### Run the test notebooks in Jupyter Lab

```bash
cd teeport-test
jupyter lab
```

In the newly opened tab in your browser, double click the `basic-tests.ipynb` file in the file panel to open it in the Lab. Then just follow the instructions in the notebook to get the basic ideas of how to use Teeport.
