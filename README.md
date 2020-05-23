# ubuntu-cuda
CUDA libraries and header files for Ubuntu

JetPack 4.3 contents:
- CUDA version 10.0

## How to extract CUDA libraries from JetPack

These were the steps taken to extract and repackage the CUDA libraries.

### Download JetPack 4.3 libraries.

We have 2 options here:
1. Download from the direct links below:
   -  https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/cuda-repo-cross-aarch64-10-0-local-10.0.326_1.0-1_all.deb
   -  https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/cuda-repo-ubuntu1804-10-0-local-10.0.326-410.108_1.0-1_amd64.deb
2. Download SDK Manager from https://developer.nvidia.com/nvidia-sdk-manager
   - Install the SDK Manager. It is usually installed here: `/opt/nvidia/sdkmanager/sdkmanager-gui`
   - Use it to download JetPack 4.3 for Jetson Nano. At the time of this writing, it has a bug that prevents it from installing the packages automatically.

### Install the downloaded packages

1. Manually install the downloaded packages. The downloaded packages are usually found in `~/Downloads/nvidia/sdkm_downloads` directory.
2. On x86_64 host, install `cuda-repo-cross-aarch64-10-0-local-10.0.326_1.0-1_all.deb` and `cuda-repo-ubuntu1804-10-0-local-10.0.326-410.108_1.0-1_amd64.deb`
   - `sudo dpkg --force-all -i cuda-repo-cross-aarch64-10-0-local-10.0.326_1.0-1_all.deb`
   - `sudo dpkg --force-all -i cuda-repo-ubuntu1804-10-0-local-10.0.326-410.108_1.0-1_amd64.deb`
   - `sudo apt-key add /var/cuda-repo-10-0-local-10.0*/*.pub'`
   - `sudo apt-get -y update`
   - `sudo apt install -y gnupg libgomp1 libfreeimage-dev libopenmpi-dev openmpi-bin`
   - `sudo apt-get -y --allow-downgrades install cuda-toolkit-10-0 cuda-cross-aarch64-10-0`
   - Development files will be installed in `/usr/local/cuda-10.0` directory.

### Compress the libraries

1. On x86_64 host, compress CUDA core libraries
   - `XZ_OPT=-9 tar -C / -cJf cuda-10.0-ubuntu1804.tar.xz usr/local/cuda-10.0`
