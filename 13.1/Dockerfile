FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

ADD Quartus-web-13.1.0.162-linux.tar /quartus

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
                           xterm:amd64 \
                           libc6-i386 && \
	dpkg --add-architecture i386 && \
	apt-get -y -qq update && \
    apt-get -y -qq install	libc6:i386 \
							libncurses5:i386 \
							libstdc++6:i386 \
							libpng12-0:i386 \
							libfreetype6:i386 \
							libsm6:i386 \
							libxrender1:i386 \
							libfontconfig1:i386 \
							libxext6:i386 \
							libxtst6:i386 \
							libxi6:i386 \
							libgtk2.0-0:i386 && \
	ln -s /usr/bin/env /bin/env

# create a normal user so we're not running as root
RUN export uid=1000 gid=1000 && \
    mkdir -p /home/developer && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer && \
	chmod a+w /opt

# switch to user so it installs from the user's context
USER developer
ENV HOME /home/developer

CMD /opt/altera/quartus/bin/quartus
