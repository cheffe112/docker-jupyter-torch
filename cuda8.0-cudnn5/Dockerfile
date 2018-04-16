FROM bethgelab/jupyter-torch:cuda8.0-cudnn5

MAINTAINER Tobias Fromm <t.fromm@jacobs-university.de>

# This is a fix for bethgelab/jupyter-deeplearning-x:cuda8.0-cudnn5 which install iTorch into /usr/local instead of usr/local.

RUN git clone https://github.com/facebook/iTorch.git /usr/local/itorch \
  && cd /usr/local/itorch \
  && luarocks make \
  && cp -r /root/.ipython/* /usr/local/share/jupyter/
