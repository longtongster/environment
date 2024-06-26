# BUILD THE IMAGE: 
# docker image build . -t my-example --build-arg="USER_NAME=tommy" --build-arg="USER_ID=1001" -f .\docker\Dockerfile
#
# RUN CONTAINER - this runs the main.py script 
# docker run --rm my-example (if an entry point is set)
# docker container run --rm -it my-example bash (if no entry point is set)
# docker container run -it --entrypoint /bin/bash my-example overwrite entry point
#
# REFERENCES: source to get activate conda virtual env in container:
# https://pythonspeed.com/articles/activate-conda-dockerfile/
#
# REMARK - if log into the container and then try `conda activate sacha` I got an error message.
# The following solves the issue
# $ source activate base
# $ conda activate sacha
#
# RUN notebook
# - REMOVE the entrypoint below and set last command as `USER root`
# - build the image
# - run
# docker run -i -t -p 8888:8888 my-example /bin/bash -c "conda install jupyter -y --quiet &&   mkdir -p /opt/notebooks &&    jupyter notebook  --notebook-dir=/opt/notebooks --ip='*' --port=8888  --no-browser --allow-root"
# Then http://localhost:8888 to find the notebook. Use the token that the container outputs to log into the noteboook ... tada
#
# change the default shell of a user to /bin/bash
# usermod --shell /bin/bash tommy


# changed below to use anaconda3 image instead of python:3.10-slim
FROM continuumio/anaconda3

# TO BE DONE - build arguments to create  user
ARG USER_ID
ARG USER_NAME

# send stdout an stderr directly to terminal (logs) 
ENV PYTHONUNBUFFERED=1

# update and upgrade linux in the container
RUN apt-get update -y && apt-get upgrade -y

WORKDIR /app

# environment varaible used to create home directory of user.
ENV HOME=/home/${USER_NAME}

# create a system user group - use `cat /etc/group` to see that the group is created
RUN addgroup --system --gid ${USER_ID} ${USER_NAME}

# create a user and it the group created
# the -m flag creates a home direcotry 
RUN useradd --system -m --no-log-init --home-dir ${HOME} --uid ${USER_ID} --gid ${USER_NAME} --groups ${USER_NAME} ${USER_NAME}

# change owernship of the HOME directory to the created user
# remark: running it with and without the command below it seems the ownership is already there
RUN chown -R ${USER_NAME}:${USER_NAME} ${HOME}

# create a directory and change ownership of the /app and /tmp folders
#RUN mkdir -p /app 
RUN chown -R ${USER_NAME}:${USER_NAME} /app /tmp

USER ${USER_NAME}

# Create the environment:
COPY environment.yml .
RUN conda env create -f environment.yml

# Make RUN commands use the new environment:
# SHELL ["conda", "run", "-n", "sacha", "/bin/bash", "-c"]

# Demonstrate the environment is activated:
#RUN echo "Make sure flask is installed:"
#RUN python -c "import flask"

# The code to run when container is started:
# Note that the code is run from the environment "sacha"
COPY main.py .
# USER root  # - uncomment to root if you want to start jupyter notebook

# use conda to run main.py in the virtual environment 'sacha'
ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "sacha", "python", "main.py"]