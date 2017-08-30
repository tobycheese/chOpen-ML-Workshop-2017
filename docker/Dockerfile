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
RUN apt-get update && apt-get install -y curl git mmv vim-tiny wget

# install generic build dependencies
RUN apt-get update && apt-get -y install make cmake gcc g++ gfortran

# install computing libraries
RUN apt-get update && apt-get install -y libopenblas-dev liblapack-dev 

####################################################################################################

# Create homedir and user
RUN useradd --create-home $user

# install cpython 3 as well as pip and virtualenv
RUN apt-get update && apt-get install -y python3 python3-dev python3-pip

# install jupyter with notebook
RUN pip3 install --upgrade pip
RUN pip3 install jupyter jupyter_contrib_nbextensions

# install 'generic' machine learning and other useful stuff
RUN pip3 install numpy scipy pandas scikit-learn ml_metrics h5py Pillow matplotlib seaborn joblib more_itertools

####################################################################################################

# install stuff needed for toy projects

# google cloud sdk
RUN echo "deb http://packages.cloud.google.com/apt cloud-sdk-stretch main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN apt-get update && apt-get install -y google-cloud-sdk google-cloud-sdk-app-engine-python

# install dlib from source, needed for face_recognition
RUN apt-get update && apt-get install -y libboost-all-dev
RUN git clone https://github.com/davisking/dlib.git
WORKDIR dlib
RUN mkdir build && cd build && cmake .. && cmake --build . --config Release
RUN python3 setup.py install
WORKDIR ..
RUN rm -r dlib

# all necessary python libs
RUN pip3 install flickrapi google-cloud-vision face_recognition plotly bokeh folium ggplot \
                 mlxtend 

####################################################################################################

# configure notebook
USER $user
RUN mkdir /home/$user/.jupyter/ && jupyter notebook --generate-config && \
    jupyter contrib nbextension install --user && \
    jupyter nbextension enable exercise2/main
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
