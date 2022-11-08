# build this image docker build xterm .
# run this image:
# docker run --rm  -v ${HOME}:${HOME} -v /tmp/.X11-unix:/tmp/.X11-unix \
# -e DISPLAY -e "QT_X11_NO_MITSHM=1" xterm
#
FROM ubuntu:20.04
RUN apt-get -y update
RUN apt-get -y install xterm
RUN apt-get -y install xauth
RUN apt-get -y install sudo

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

CMD xterm
