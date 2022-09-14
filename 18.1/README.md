Build an image for Quartus-Lite 18.1 for cyclone IV family.

* copy the files cyclone-18.1.0.625.qdz and QuartusLiteSetup-18.1.0.625-linux.run from the distribution tarfile

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
