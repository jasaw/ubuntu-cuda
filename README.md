# ubuntu-cuda
CUDA libraries and header files for Ubuntu

This project aims to provide a convenient way to download all CUDA files needed for cross compiling CUDA code on an x86_64 Ubuntu host for aarch64 target.

JetPack 4.6 Rev 2 contents:
- CUDA version 10.2.460

## How to extract CUDA libraries from JetPack

These were the steps taken to extract and repackage the CUDA libraries.

### Download JetPack 4.6 libraries.

We have 2 options here:
1. Download from the direct links below:
   -  https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.6/JETPACK_46_b194/cuda-repo-cross-aarch64-ubuntu1804-10-2-local_10.2.460-1_all.deb
   -  https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.6/JETPACK_46_b194/ubuntu1804/cuda-repo-ubuntu1804-10-2-local_10.2.460-450.115-1_amd64.deb
2. Download SDK Manager from https://developer.nvidia.com/nvidia-sdk-manager
   - Install the SDK Manager. It is usually installed here: `/opt/nvidia/sdkmanager/sdkmanager-gui`

### Install the downloaded packages

1. Manually install the downloaded packages. The downloaded packages are usually found in `~/Downloads/nvidia/sdkm_downloads` directory.
2. On x86_64 host, install `cuda-repo-cross-aarch64-10-2-local-10.2.460_1.0-1_all.deb` and `cuda-repo-ubuntu1804-10-2-local-10.2.460-450.115_1.0-1_amd64.deb`
   - `sudo dpkg --force-all -i cuda-repo-cross-aarch64-10-2-local-10.2.460_1.0-1_all.deb`
   - `sudo dpkg --force-all -i cuda-repo-ubuntu1804-10-2-local-10.2.460-450.115_1.0-1_amd64.deb`
   - `sudo apt-key add /var/cuda-repo-10-2-local-10.2*/*.pub'`
   - `sudo apt-get -y update`
   - `sudo apt install -y gnupg libgomp1 libfreeimage-dev libopenmpi-dev openmpi-bin`
   - `sudo apt-get -y --allow-downgrades install cuda-toolkit-10-2 cuda-cross-aarch64-10-2`
   - Development files will be installed in `/usr/local/cuda-10.2` directory.

### Compress the libraries

1. On x86_64 host, compress CUDA core libraries
   - `XZ_OPT=-9 tar -C / -cJf cuda-10.2-ubuntu1804.tar.xz usr/local/cuda-10.2`



### Files too big, split and reassemble

To split, run: `split -b 1800M -a 1 --numeric-suffixes=0 cuda-10.2-ubuntu1804.tar.xz cuda-10.2-ubuntu1804.tar.xz.part`

To reassemble, run: `cat cuda-10.2-ubuntu1804.tar.xz.part? > cuda-10.2-ubuntu1804.tar.xz`
