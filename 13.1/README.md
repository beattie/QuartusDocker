Build an image for Quartus-Web 13.1 for cyclone IV family.

Because the 13.1 installer needs an X11 server this is currently a multiple
step process, with user intervention. The make file will build an image
with the Quartus distribution. This image should be run _see below_
the example below will run the installer and then Quartus to answer the startup
question so it will not be presented every time Quartus is started.

The second step of the *image_built* target will run the Quartus installer in
the intermediate container and the start Quartus. The installer runs in unattended mode but does require the Xserver to display progress bars (why i don't
know). After the installer completes, it will run Quartus. The first run of
Quartus asks a question that I find annoying. Two questions are asked the first
one answer as you like, the second has three options, select
**Run the Quartus...** Answer this question and then exit Quartus. Following
completion run **docker commit** which will create a docker image (which will
not ask the annoying startup question).

The final docker image can be run with the **quartus** command:
~~~~
quartus () {
	if [ x$1 == "x" ] ; then
		echo "quartus version must be specified!"
    exit 1
	else
		image=quartus:$1
	fi
	docker run --rm -v /sys:/sys:ro -v ${HOME}:${HOME} \
		-v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY \
		-e "QT_X11_NO_MITSHM=1" --privileged \
		-v /dev/bus/usb:/dev/bus/usb ${image}
}
~~~~
The image _quartus-installer:13.1__ and the container _quartus-base-container_
can now be removed.

**NB: the message "You must have the 32bit..." is an artifact of using Ubuntu
which the installer does not understand and can safely be ignored**
