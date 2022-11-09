Build an image for Quartus-Lite 15.1

-----------------------------
* to run the image the following bash file (run) can be used:
~~~~
docker run --rm -it  -v ${HOME}:${HOME} -v /tmp/.X11-unix:/tmp/.X11-unix \
	-v /sys:/sys:ro -e DISPLAY -e "QT_X11_NO_MITSHM=1" --privileged \
	quartus:15.1	
}
~~~~
If you find the startup question anoying like I do, do the following (for some reason I can't
do this from make.):
~~~~
docker tag quartus:15.1 quartus:intermediate
docker run --name quartus-base-container -v /sys:/sys:ro \
		-v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY \
		-e "QT_X11_NO_MITSHM=1" --privileged \
		-v /dev/bus/usb:/dev/bus/usb quartus:15.1
docker rmi quartus:15.1
docker commit quartus-base-container quartus:15.1
docker rm quartus-base-container
docker rmi quartus:intermediate
~~~~
