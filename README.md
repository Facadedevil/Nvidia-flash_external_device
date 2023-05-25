# bootFromExternalStorage
<b>These scripts is to help flash using external device like ssd, m.2, sdcard</b>

Shell scripts to setup a NVIDIA Jetson Xavier or Orin (AGX, NX, Nano models) to boot from external storage.

Installs JetPack 5.1.1, L4T 35.3.1 on the Jetson Developer Kit

_**Warning for the Jetson Xavier NX and AGX Xavier:** There is an issue with the USB stack. You cannot boot from USB on these systems._

_**Note for the Jetson Xavier NX:** For a Jetson AGX Xavier system, the board must be initially flashed to eMMC before using this method._

_**Note for the Jetson Xavier NX:** Remove the SD card for this process. Also, there is flash memory onboard the Xavier NX module, QSPI-Nor.  This script flashes the QSPI memory in addition to the disk image._

_**Note for the Jetson Orin Nano and Orin NX:** There is flash memory onboard the Orin modules, QSPI-Nor.  This script flashes the QSPI memory in addition to the disk image._

Around 40GB of free space is needed on the host for these scripts and Jetson disk image files. More is better.

On the host machine, follow this sequence:
1. `get_jetson_files.sh` - Downloads the Jetson BSP and sample rootfs, copies NVIDA user space libraries to rootfs. Also installs dependencies needed to flash the Jetson.
2. `flash_jetson_external_storage.sh` - Flash the Jetson (make sure that the Jetson is connected via USB, external storage is attached to the Jetson and that the Jetson is in Force Recovery Mode)

`install_jetson_default_packages.sh` to install the standard JetPack packages.

## Scripts

### get_jetson_files.sh
Downloads the Jetson BSP and rootfs for the Xavier/Orin Dev Kits. This script must be run on the host machine.

### install_dependencies.sh
Install dependencies for flashing the Jetson. This script must be run on the host machine. This is done by get_jetson_files.sh, but is added here as legacy.

### flash_jetson_external_storage.sh
Flashes the Jetson attached to the host via a USB cable. This script must be run on the host machine. The Jetson must be in Force Recovery Mode.
The script prepares external storage attached to the Jetson, either NVMe or USB. Default is NVMe as the Orin and Xaviers have M.2 Key M slots which accept NVMe SSDs. The SSDs must be PCIE, SATA does not work. For the Xavier NX and Orin NX and Orin Nano, this flashes the QSPI memory on the Jetson module.
```
Usage: ./flash_jetson_external_storage [OPTIONS]
  No option flashes to nvme0n1p1 by default
  -s | --storage - Specific storage media to flash; sda1 or nvme0n1p1
  -h | --help    - displays this message
```
 
 ### install_jetson_default_packages.sh
 Once the script flash_jetson_external_storage script completes, the Jetson is ready for setup. First, configure the Jetson through the standard oem-config process. Then you are ready to install the default JetPack packages using this script.  You will need to either download the script or clone this repository on the Jetson itself. The Jetson must be connected to the Internet.
 
 Executing the script will install the metapackage nvida-jetpack which in turn installs the following metapackages:
 
 * nvidia-cuda
 * nvidia-cudnn
 * nvidia-tensorrt
 * nvidia-visionworks
 * nvidia-vpi
 * nvidia-l4t-jetson-multimedia-api
 * nvidia-opencv
 
 The script installs other packages, to match the default SD Card installation. These include:
 
 * libtbb-dev
 * uff-converter-tf
 * python3-vpi1
 * python3-libnvinfer-dev
 * Various Python2.7 support files
