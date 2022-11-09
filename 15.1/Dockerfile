# build this image docker build xterm .
# run this image:
# docker run --rm -i -t  -v ${HOME}:${HOME} buildroot:18.04 bash
# or
# . ./run
#
FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

ADD Quartus-lite-15.1.0.185-linux.tar /quartus

RUN apt-get -y update
RUN apt-get -y install apt-utils
RUN apt-get -y install xterm
RUN apt-get -y install xauth
RUN apt-get -y install sudo
RUN apt-get -y install wget

RUN apt-get -y -qq install locales && locale-gen en_US.UTF-8 && \
    apt-get -y -qq install software-properties-common \
                           openjdk-9-jre \
                           libglib2.0-0:amd64 \
                           libfreetype6:amd64 \
                           libsm6:amd64 \
                           libxrender1:amd64 \
                           libfontconfig1:amd64 \
                           libxext6:amd64 \
                           libpng12-0:amd64

# create a normal user so we're not running as root
RUN export uid=1000 gid=1000 && \
    mkdir -p /home/developer && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer

# switch to user
USER developer
ENV HOME /home/developer

RUN sudo chmod a+w /opt
RUN /quartus/setup.sh  --unattendedmodeui none --mode unattended --installdir /opt/intelFPGA_lite

CMD ["/opt/intelFPGA_lite/quartus/bin/quartus", "--64bit"]
