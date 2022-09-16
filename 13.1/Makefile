INSTALLER_URL=http://download.altera.com/akdlm/software/acdsinst/13.1/162/ib_tar/Quartus-web-13.1.0.162-linux.tar
INSTALLER=Quartus-web-13.1.0.162-linux.tar
INSTALLER_CHECKSUM=BA705F9D15F3A43AB7E86D297F394EE3

image_built: ${INSTALLER}
	docker build -t quartus-installer:13.1 --build-arg DISPLAY=$(DISPLAY) .
	docker run --name quartus-base-container \
		-v /tmp/.X11-unix:/tmp/.X11-unix \
		-e DISPLAY -e "QT_X11_NO_MITSHM=1" quartus-installer:13.1 \
  		bash -c "/quartus/setup.sh --mode unattended --installdir /opt/altera && /opt/altera/quartus/bin/quartus"
	touch image_built

commit:
	docker commit --change='CMD /opt/altera/quartus/bin/quartus' quartus-base-container quartus:13.1

${INSTALLER}:
	wget $(INSTALLER_URL)
	@SUM=$$(md5sum $(INSTALLER) | awk '{print toupper($$1)}'); \
        if [ "$$SUM" != "$(INSTALLER_CHECKSUM)" ]; \
			then echo "Installer checksum doesn't match."; exit 1; fi

clean:
	rm image_built
	-docker rmi -f quartus-installer:13.1

distclean:	clean
	rm CentosOS-Base.repo
