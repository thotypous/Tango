# Autolab - autograding docker image

FROM ubuntu:14.04
MAINTAINER Mihir Pandya <mihir.m.pandya@gmail.com>

RUN apt-get update --fix-missing
RUN apt-get install -y gcc
RUN apt-get install -y make
RUN apt-get install -y build-essential
RUN apt-get install -y python3 python3-matplotlib python3-scipy python3-numpy
RUN apt-get install -y p7zip-full

# Install autodriver
WORKDIR /home
RUN useradd autolab
RUN useradd autograde
RUN mkdir autolab autograde output
RUN chown autolab:autolab autolab
RUN chown autolab:autolab output
RUN chown autograde:autograde autograde
RUN apt-get install -y git
RUN git clone https://github.com/autolab/Tango.git
WORKDIR Tango/autodriver
RUN make clean && make
RUN cp autodriver /usr/bin/autodriver
RUN chmod +s /usr/bin/autodriver

# UTF-8 Locale - https://stackoverflow.com/a/46531235
RUN export DEBIAN_FRONTEND=noninteractive; \
  apt-get update -q -q && \
  apt-get install --yes locales && \
  locale-gen --no-purge en_US.UTF-8 && \
  update-locale LANG=en_US.UTF-8 && \
  echo locales locales/locales_to_be_generated multiselect en_US.UTF-8 UTF-8 | debconf-set-selections && \
  echo locales locales/default_environment_locale select en_US.UTF-8 | debconf-set-selections && \
  dpkg-reconfigure locales
ENV LANG en_US.UTF-8

# Clean up
WORKDIR /home
RUN apt-get remove -y git
RUN apt-get -y autoremove
RUN rm -rf Tango/

# Check installation
RUN ls -l /home
RUN which autodriver
