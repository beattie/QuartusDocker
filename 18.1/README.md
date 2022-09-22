Build an image for Quartus-Lite 18.1

-----------------------------
* to run the image the following bash function can be used:
~~~~
quartus() {
  docker run --rm -v /sys:/sys:ro -v ${HOME}:${HOME} \
  -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY \
  -e "QT_X11_NO_MITSHM=1" --privileged \
  -v /dev/bus/usb:/dev/bus/usb quartus:18.1
}
~~~~
If you find the startup question like I do do the following (for some reason I can't
do this from make.):
~~~~
docker tag quartus:18.1 quartus:intermediate
docker run --name quartus-base-container -v /sys:/sys:ro \
		-v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY \
		-e "QT_X11_NO_MITSHM=1" --privileged \
		-v /dev/bus/usb:/dev/bus/usb quartus:18.1
docker rmi quartus:18.1
docker commit quartus-base-container quartus:18.1
docker rm quartus-base-container
docker rmi quartus:intermediate
~~~~
