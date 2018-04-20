FROM nvidia/cuda:8.0-devel

MAINTAINER Tobias Fromm <t.fromm@jacobs-university.de>

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

# Torch7 installation scripts - http://torch.ch/docs/getting-started.html
RUN git clone https://github.com/torch/distro.git torch --recursive && \
    cd torch && \
    bash install-deps && \
    ./install.sh

ENV PATH=/root/torch/install/bin:$PATH

# Install miniconda
ENV CONDA_DOWNLOAD_SCRIPT Miniconda3-latest-Linux-x86_64.sh
ENV CONDA_DIR /opt/conda

RUN wget -q https://repo.continuum.io/miniconda/$CONDA_DOWNLOAD_SCRIPT && \
    bash $CONDA_DOWNLOAD_SCRIPT -b -p $CONDA_DIR && \
    rm $CONDA_DOWNLOAD_SCRIPT

ENV PATH $CONDA_DIR/bin:$PATH

# Install jupyter
RUN conda install -y jupyter

# Install iTorch
RUN git clone https://github.com/facebook/iTorch.git && \
    cd iTorch && \
    luarocks make

WORKDIR /root/dev

# update some packages
RUN conda update -n base conda
RUN conda update jupyter

# manual pytorch install from https://github.com/pytorch/pytorch#from-source
# - need to compile pytorch from source because otherwise old GPUs like Geforce 660 cannot be used anymore
RUN export CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"
# pytorch installation from package:
#RUN pip install torch torchvision

# Install basic dependencies
RUN conda install numpy pyyaml mkl mkl-include setuptools cmake cffi typing

# Add LAPACK support for the GPU
RUN conda install -c pytorch magma-cuda80

RUN git clone --recursive https://github.com/pytorch/pytorch && \
    cd pytorch && \
    python setup.py install

RUN git clone --recursive https://github.com/pytorch/vision && \
    cd vision && \
    python setup.py install

# install matplotlib
RUN pip install matplotlib

# fix some weird error regarding an outdated dateutil version
RUN pip install python-dateutil --upgrade

# Default command is to run the itorch-notebook
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--no-browser", "--allow-root"]
