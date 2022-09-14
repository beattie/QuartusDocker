# ubuntu 18.04 is officially supported by this version of quartus
# libpng12 included in 16.04 but not in 18.04, so we use 16.04 for convenience
FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

ARG REALUSER

ARG QUARTUS=QuartusLiteSetup-18.1.0.625-linux.run
ARG CYCLONE=cyclone-18.1.0.625.qdz

COPY $QUARTUS /$QUARTUS
COPY $CYCLONE /$CYCLONE

RUN apt-get update && \
    apt-get -y -qq install apt-utils sudo && \
        apt-get -y -qq install locales && locale-gen en_US.UTF-8 && \
    apt-get -y -qq install software-properties-common \
                           openjdk-9-jre \
                           libglib2.0-0:amd64 \
                           libfreetype6:amd64 \
                           libsm6:amd64 \
                           libxrender1:amd64 \
                           libfontconfig1:amd64 \
                           libxext6:amd64 \
                           libpng12-0:amd64 \
                           xterm:amd64 && \
    chmod 755 /$QUARTUS

# create a normal user so we're not running as root
RUN export uid=1000 gid=1000 && \
    mkdir -p /home/developer && \
    mkdir -p /home/$REALUSER && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer

# switch to user so it installs from the user's context
USER developer
ENV HOME /home/developer
# install quartus as the user (not root)
RUN sudo mkdir -p /opt/intelFPGA_lite && \
	sudo chmod a+w /opt/intelFPGA_lite && \
	/$QUARTUS --mode unattended --installdir /opt/intelFPGA_lite --accept_eula 1 && \
    sudo rm -f /$QUARTUS && \
    sudo rm -f /$CYCLONE

# run from xterm to capture any stdio logging (not sure there is any, but can't hurt)
#CMD xterm -e "/home/developer/altera_lite/quartus/bin/quartus --64bit"
CMD ["/opt/intelFPGA_lite/quartus/bin/quartus", "--64bit"]
