FROM jacobsrobotics/docker-jupyter-torch:basic

MAINTAINER Tobias Fromm <t.fromm@jacobs-university.de>

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
