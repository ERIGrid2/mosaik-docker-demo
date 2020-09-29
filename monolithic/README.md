# Monolithic mosaik-docker simulation setup

## Overview

The setup contains the following files:

 * [`main.py`](./main.py): the mosaik scenario file
 * [`mosaik-docker.json`](./mosaik-docker.json): the mosaik-docker simulation setup configuration file
 * [`dockerfiles/Dockerfile_main`](./dockerfiles/Dockerfile_main): defines the monolithic Docker image for this simulation setup
 * additional data: [`demo_lv_grid.json`](./demo_lv_grid.json), [`data/profiles.data.gz`](./data/profiles.data.gz) and [`data/pv_10kw_1week.csv`](./data/pv_10kw_1week.csv)

### Simulation setup configuration

The simulation setup configuration is stored in JSON format (file [`mosaik-docker.json`](./mosaik-docker.json)).
It defines the following features for this simulation setup:
 * **scenario_file**:
   specifies which file will be used as mosaik scenario file (file [`main.py`](./main.py))
 * **docker_file**:
   specifies the main Dockerfile [`dockerfiles/Dockerfile_main`](./dockerfiles/Dockerfile_main) for this simulation setup
 * **extra_files**:
   add file [`demo_lv_grid.json`](./demo_lv_grid.json) to the working directory of the Docker container
 * **extra_dirs**:
   add the directory [`./data`](./data) and all its contents to the working directory of the Docker container
 * **results**:
   define the output of the simulation (file `demo.hdf5`), which can be retrieved after the simulation has finished successfully

### Docker image definition

Because this example follows a *monolithic approach*, this setup comprises only a single Dockerfile to which the mosaik scenario file, all simulators and the additional data is added.

The first 3 lines are boilerplate code that should be added to each main Dockerfile.
They define the base image (that comes with mosaik installed) and defines the build-time variables `SCENARIO_FILE` and `EXTRA`:

```
FROM mosaik/orch-base:v1
ARG SCENARIO_FILE
ARG EXTRA
```

The next lines add all Python packages that are required to run the simulators:
```
RUN pip install mosaik-csv==1.0.3
RUN pip install mosaik-hdf5==0.3
RUN pip install mosaik-householdsim==2.0.3
RUN pip install mosaik-pypower==0.7.2
RUN pip install networkx==2.4
```

The last 3 lines are also boilerplate code that should be added to each main Dockerfile.
They copy all required resources (mosaik scenario file, additional files and directories) and define the command that will be executed to run the simulation.:
```
COPY $SCENARIO_FILE .
COPY $EXTRA .
ENTRYPOINT python $SCENARIO_FILE
```

## Running the example from the command line

In the command terminal change to folder `monolithic`, then run the following commands.
More information in `mosaik-docker`'s command line interface (CLI) can be found in the [documentation](https://mosaik-docker.readthedocs.io/en/latest/cli-reference.html).

Check that the simulation setup is valid:
```
check_sim_setup .
```

Build the simulation setup's Docker image:
```
build_sim_setup .
```

Start a simulation (with simulation ID *TEST-SIM*):
```
start_sim . TEST-SIM
```

Check the status of the simulation:
```
get_sim_status .
```

Once the simulation is finished, retrieve the results:
```
get_sim_results . --id TEST-SIM
```

The simulation results (file `demo.hdf5`) will be copied to a subfolder with the same name as the simulation ID:
```
cd TEST-SIM
h5stat demo.hdf5
```
