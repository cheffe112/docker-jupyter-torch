FROM dhunter/itorch-notebook

MAINTAINER Tobias Fromm <t.fromm@jacobs-university.de>

RUN pip install --upgrade pip

# install matplotlib
RUN pip install matplotlib

# fix some weird error regarding an outdated dateutil version
RUN pip install python-dateutil --upgrade

# install pytorch
RUN pip install torch torchvision
