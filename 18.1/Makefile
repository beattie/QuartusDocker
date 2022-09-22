URL=http://download.altera.com/akdlm/software/acdsinst/18.1std/625/ib_tar/Quartus-lite-18.1.0.625-linux.tar
INSTALLER=Quartus-lite-18.1.0.625-linux.tar
INSTALLER_CHECKSUM=EAF82392603B92DAE632CC0F356B08AA

image_built: ${INSTALLER}
	docker build -t quartus:18.1 .
	touch image_built

$(INSTALLER):
	wget $(URL)
	@SUM=$$(md5sum $(INSTALLER) | awk '{print toupper($$1)}'); \
		if [ "$$SUM" != "$(INSTALLER_CHECKSUM)" ]; then echo "Checksum doesn't match."; exit 1; fi

clean:
	rm -f image_built image_fix
	-docker rmi -f quartus:18.1
