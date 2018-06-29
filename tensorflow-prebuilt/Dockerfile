FROM tensorflow/tensorflow:latest-gpu

MAINTAINER Tobias Doernbach <t.fromm@jacobs-university.de>

# Install git and iTorch dependencies
RUN apt-get update && apt-get install -y \
    sudo \
    python-pip \
    software-properties-common \
    git \
    wget \
    libzmq3-dev \
    libssl-dev \
    python-zmq

RUN pip install --upgrade pip

WORKDIR /root

# Install miniconda
ENV CONDA_DOWNLOAD_SCRIPT Miniconda3-latest-Linux-x86_64.sh
ENV CONDA_DIR /opt/conda

RUN wget -q https://repo.continuum.io/miniconda/$CONDA_DOWNLOAD_SCRIPT && \
    bash $CONDA_DOWNLOAD_SCRIPT -b -p $CONDA_DIR && \
    rm $CONDA_DOWNLOAD_SCRIPT

ENV PATH $CONDA_DIR/bin:$PATH

WORKDIR /root/dev

# update some packages
RUN conda update -n base conda

# install matplotlib
RUN [ "/bin/bash", "-c", "pip install matplotlib"]

# use Anaconda environments inside Jupyter
RUN conda install numpy scipy pyyaml mkl mkl-include setuptools cmake cffi typing nb_conda jupyter

# Default command is to run the itorch-notebook
CMD ["/bin/bash", "-c", "jupyter notebook --ip=0.0.0.0 --no-browser --allow-root"]
#CMD /bin/bash -c "tail -f /dev/null"
