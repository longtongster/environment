# Introduction

In quite some cases the same packages are required in a virtual conda environment. For this an environment.yml file was created with the basis packages that are always required. This file can be adjusted to include different packages specific for your need.

## Create environment

Run the following command the create a conda environment

`conda env create --file environment.yml`

## Remove environment

To remove an environment run:

`conda remove -n ENV_NAME --all`
