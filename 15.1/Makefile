URL=https://download.altera.com/akdlm/software/acdsinst/15.1.2/193/ib_tar/Quartus-lite-15.1.2.193-linux.tar
INSTALLER=Quartus-lite-15.1.2.193-linux.tar
INSTALLER_CHECKSUM=0F5CF26B7BB1837DA350F847DC8C98D8

all: container-image-built

${INSTALLER}:
	curl -O $(URL)
	echo "Comparing checksum..."
	@SUM=$$(md5sum $(INSTALLER) | awk '{print toupper($$1)}'); \
		if [ "$$SUM" != "$(INSTALLER_CHECKSUM)" ]; then echo "Installer checksum doesn't match."; exit 1; fi
