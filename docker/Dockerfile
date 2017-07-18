FROM debian:stretch-slim
LABEL maintainer "tobycheese https://github.com/tobycheese/"

# desired username
ARG user=johndoe

####################################################################################################

# install apt-utils to allow package configuration
RUN apt-get update && apt-get install -y apt-utils

# avoid nasty unable to initialize frontend errors
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# install base packages
RUN apt-get update && apt-get install -y git mmv vim-tiny wget

# install generic build dependencies
RUN apt-get update && apt-get -y install make cmake gcc g++ gfortran

####################################################################################################

# Create homedir and user
RUN useradd --create-home $user

# install cpython 3 as well as pip and virtualenv
RUN apt-get update && apt-get install -y python3 python3-dev python3-pip

# install jupyter with notebook
RUN pip3 install --upgrade pip
RUN pip3 install jupyter

# install 'generic' machine learning and other useful stuff
RUN pip3 install numpy scipy pandas scikit-learn ml_metrics h5py Pillow matplotlib seaborn joblib more_itertools

# install stuff needed for toy projects
RUN pip3 install flickrapi

####################################################################################################

# configure notebook
USER $user
RUN mkdir /home/$user/.jupyter/ && jupyter notebook --generate-config
WORKDIR /home/$user/.jupyter/
RUN echo "c.NotebookApp.open_browser = False" >> jupyter_notebook_config.py && \
    echo "c.JupyterApp.answer_yes = True" >> jupyter_notebook_config.py && \
    echo "c.NotebookApp.ip = '*'" >> jupyter_notebook_config.py && \
    echo "c.NotebookApp.allow_origin = '*'" >> jupyter_notebook_config.py && \
    echo "c.NotebookApp.token = ''" >> jupyter_notebook_config.py && \
    echo "c.NotebookApp.password = ''" >> jupyter_notebook_config.py


# Create dir for external data
RUN ln -s /data /home/$user/toyprojects

####################################################################################################

# cleanup to make image smaller (does this really help?)
USER root
RUN apt-get autoremove -y && apt-get clean
RUN rm -r /root/.cache/

WORKDIR /home/$user/toyprojects/
EXPOSE 8888
USER $user
# looks like tini really isn't needed anymore when running container with --init
CMD /usr/local/bin/jupyter-notebook -y $user
