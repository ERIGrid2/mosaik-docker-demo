# mosaik-docker-demo

This repository contains two simulation setups that demonstrate the deployment of the [mosaik co-simulation framework](https://mosaik.offis.de/) with [Docker](https://docs.docker.com/).
To run these examples, packages [mosaik-docker](https://mosaik-docker.readthedocs.io/en/latest/installation.html) and [Docker](https://docs.docker.com/get-docker/) have to be installed.
For running the examples from JupyterLab, package [mosaik-docker-jl](https://mosaik-docker.readthedocs.io/projects/jupyter/en/latest/installation.html) needs to be installed.

## About

To set up a [dockerized](https://www.docker.com/resources/what-container) mosaik simulation setup you need to provide the following:

 * add a mosaik scenario that
   1. starts the simulators
   2. instantiates models within the simulators
   3. connects the model instances of different simulators to establish the data flow between them
 * add a Dockerfile for running the mosaik scenario
 * add Dockerfiles for running the simulators (optional)
 * add additional resources, e.g., data files (optional)
 * add a mosaik-docker simulation setup configuration

## Monolithic vs. distributed containerization
 
There are two approaches for containerizing a mosaik co-simulation:

 1. **Monolithic approach**: 
   The mosaik scenario file, all simulators and all aditional resources can be added to a single Docker image.
   When running a simulation, a single container is instantiated from this image and executed on its own.
 2. **Distributed approach**: 
   The mosaik scenario file and the simulators can be added to separate Docker images.
   When running a simulation, an individual container is instantiated from each of these images.
   At runtime, these containers are executed in parallel and communicate with each other (via the [mosaik API](https://mosaik.readthedocs.io/en/latest/mosaik-api)).

## Examples

Two simple examples based on the [mosaik demo](https://gitlab.com/mosaik/mosaik-demo) are available:

 * A **monolithic setup** is available in folder [`monolithic`](./monolithic).
 * A **distributed setup** is available in folder [`distributed`](./distributed).
