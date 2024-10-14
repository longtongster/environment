# Introduction

A good source is 

`https://medium.com/hydroinformatics/how-to-make-your-python-environment-reproducible-common-practices-conda-environment-de28195b74de`

This post also explains more about `mamba` since conda is oh so slow...

PART 1:

In quite some cases the same packages are required in a virtual conda environment. For this an environment.yml file was created with the basis packages that are always required. This file can be adjusted to include different packages specific for your need.

PART 2:
The folder `docker/Dockerfile` gives instructions to build an image from anaconda3. It creates a user, a virtual conda environment and runs a very simple script using the enviroment. 

## Create environment

Run the following command the create a conda environment

`conda env create --file environment.yml`

## Remove environment

To remove an environment run:

`conda remove -n ENV_NAME --all`

## Update an environment

You can update the `environment.yml` and then run

`conda env update -n ENV_NAME --file environment.yml`

this will update the environment.

## List revisions

Conda keeps track environment revisions.

`conda list --revision`

you can revert to a previous state using 

`conda install --revision <revision-number>`

where <revision-number> is the revision you want to turn back to.

The command will first display the packages that were installed after this revision and will be removed to go back to what the environment was, immediately before.

Then the command will display all the packages that will be removed (the previous package in the environment.yml file and also their dependencies)

**remark**:
In some cases installation with conda are not always succesfull. Leaving them in the current environment might pollute your environment. This is a clear way to go back.

## Docker image

An example to build the image `my-example` that creates a user `tommy` run the following command.

`docker image build . -t my-example --build-arg="USER_NAME=tommy" --build-arg="USER_ID=1001" -f .\docker\Dockerfile`

## Docker run

You can play around with the Dockerfile, build slightly different images and run the container in slightly different ways. 

`docker run --rm my-example` - if the entry point is set

`docker container run --rm -it my-example bash` - if no entry point is set

`docker container run -it --entrypoint /bin/bash my-example`  - this command overwrites an exising entrypoint
