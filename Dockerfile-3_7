# A Dockerfile to build an OpenShift Jenkins Slave Agent.
# It's based on the OpenShift Base image and provides Python 3.6.

# The v3.7 digest...
FROM openshift/jenkins-slave-base-centos7@sha256:3059d49e6666c0bc6198d20e956a5b03589bab6e06e26906c19c64ed55922754
MAINTAINER Alan Christie (alanbchristie)

RUN yum -y install yum-utils
RUN yum -y groupinstall development
RUN yum -y install https://centos7.iuscommunity.org/ius-release.rpm
RUN yum -y install \
    python36u \
    python36u-pip \
    python36u-devel

RUN update-alternatives --install /usr/bin/python python /usr/bin/python2 1
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2

RUN alternatives --install /usr/bin/pip pip /usr/bin/pip3.6 10

RUN pip install --upgrade pip

# To fix the error: -
#       UnicodeDecodeError: 'ascii' codec can't decode byte 0xc3
#                in position 3516: ordinal not in range(128)
# Seen when trying to install pyroma v2.3.
# We ned to set `LC_ALL` and allow utc-8.
ENV LC_ALL=en_US.UTF-8

# We're root at this stage of the script.
# Back to the base-image user (we know this empirically).
USER 1001

# Adjust path for '--user' use of pip
# where stuff gets installed in ${HOME}/.local/bin
ENV PATH ${PATH}:${HOME}/.local/bin

WORKDIR ${HOME}
