# Teeport Test

This serves as a guide to deploy the Teeport platform and test the basic features of Teeport.

## Prerequisites

Check if the **conda** and **docker** environments are available on the computer.

This guide has been tested on conda 4.8.3 and docker 19.03.13.

For conda, I didn't test if the slightly old versions could work, but I assume so since I didn't use any specific new features of it.

For docker, if you want to test the optional frontend part of Teeport, then you'll need docker 19.03 or higher (we'll use the new BuildKit mode provided since that version). However if for some reason you can't have docker 19.03 on your computer, no worries, it's not hard to work around it, just [shoot me an email](mailto:zhezhang@slac.stanford.edu).

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

### [Optional] Build and run the frontend service

#### Clone the repo

```bash
git clone https://github.com/SPEAR3-ML/teeport-frontend.git
```

#### Build the docker image

Go to the project directory:

```bash
cd teeport-frontend
```

Create a file named `.env` with the content below:

```
REACT_APP_URI_TASK_SERVER=ws://{IP}:8080
REACT_APP_BASENAME=/
```

Where `{IP}` should be replaced by the LAN IP of the Teeport backend service you just setup, typically something like `10.0.0.172`. You can check it with `ifconfig` on Mac/Linux or `ipconfig` on Windows.

Then in the subdir `server`, create a file named `.env` with the content below:

```
MODE=production
BASENAME=/
PORT=3000
```

After that the project file structure should look like:

```
|--teeport-frontend
    |--server
        |--.env
        |--...
    |--.env
    |--...
```

We'll use the `Dockerfile.lan` and the corresponding `Dockerfile.lan.dockerignore` to build the image, in order to do that we'll need docker 19.03 or higher, to be able to use the dockerignore file other than the default one.

Make sure you're using docker 19.03 or higher, let's enable the BuildKit mode.

If you're using bash-like terminal, run this:

```bash
export DOCKER_BUILDKIT=1
```

Else if you're using powershell:

```powershell
$Env:DOCKER_BUILDKIT = 1
```

or you're on Windows cmd:

```cmd
set DOCKER_BUILDKIT=1
```

Then build the docker image:

```bash
docker build -f Dockerfile.lan -t teeport/frontend .
```

#### Run the docker image

```bash
docker run -d -p 3000:3000 --name teeport-frontend --restart always teeport/frontend
```

Now the Teeport frontend should be accessible at port 3000. To verify that, open your favorite browser and go to [http://localhost:3000/tasks](http://localhost:3000/tasks). If you're going though this document step by step, you should be able to see the tasks created when you played with the `basic-tests.ipynb` in the last section.

Hover your mouse on the status bar (where shows the time created) of each task card will give you more options to interact with the task. Say, click on the Enter button to access the monitors of that task, you'll see the evaluation history and stuff. Try to relate the plots to what you did in the notebook to get more feelings about the mechanism of Teeport.
