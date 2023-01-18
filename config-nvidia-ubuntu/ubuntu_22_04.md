# Guide for Ubuntu 22.04

This guide will help you install and configure the NVIDIA CUDA drivers in a `Ubuntu 22.04` Linux machine to support your graphics card's compatibility and integrate it with your docker engine. This guide was based on the `NVIDIA CUDA Drivers version 12.0`.

| Operative System | Graphic Card Model |
| ---------------- | ------------------ |
| Ubuntu 22.04     | GeForce RTX 3060 Laptop GPU |

The fist step is necessary to complete the [preparation](README.md).

## NVIDIA CUDA Drivers

First, download repository information to allow the driver installation in your system by running the following command sequence:

``` BASH
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
```

Once the repository metadata is downloaded into the Ubuntu repository list, it is necessary to download the installation binaries for the system. These binaries correspond to an `x86_64` architecture. If yours is different, please check on the [official download page](https://developer.nvidia.com/cuda-downloads?) for your binary link

``` BASH
wget https://developer.download.nvidia.com/compute/cuda/12.0.0/local_installers/cuda-repo-ubuntu2204-12-0-local_12.0.0-525.60.13-1_amd64.deb
```

If the download fails, is necessary to enter the [NVIDIA CUDA Developer](https://developer.nvidia.com/cuda-toolkit-archive) site with your login and password (if you don't have it, create one) and send the command again; or download the selected version of the CUDA Drivers for your card. e.g., For this guide, the *12.0* version was chosen.

After successfully downloading the file, the following command sequence can be sent to your terminal.

``` BASH
sudo dpkg -i cuda-repo-ubuntu2204-12-0-local_12.0.0-525.60.13-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2204-12-0-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda nvidia-cuda-toolkit
```

For the official guide, take a look to the official [NVIDIA Driver](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#driver-installation) manual 

### Test the NVIDIA CUDA Drivers

If the configuration is correct, the command `nvidia-smi` should show an output similar to this one:

<p align="center">
  <img src="https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/blob/main/config-nvidia-ubuntu/img/nvidia-smi-22-04.png?raw=true" alt="NVIDIA output command"/>
</p>

If this output is shown, is it possible to continue with the [Docker NVIDIA Support](https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/tree/main/config-nvidia-ubuntu/#docker-nvidia-support)