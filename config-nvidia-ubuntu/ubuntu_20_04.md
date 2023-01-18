# Guide for Ubuntu 20.04

This guide will help you install and configure the NVIDIA CUDA drivers and the CuDNN binaries (Optional) in a `Ubuntu 20.04` Linux machine to support your graphics card's compatibility integrate it with your docker engine. This guide was based on the `NVIDIA CUDA Drivers version 11.6.0`.

| Operative System | Graphic Card Model |
| ---------------- | ------------------ |
| Ubuntu 20.04     | GeForce GTX 1060 6GB |
| Ubuntu 20.04     | GeForce GTX 1070 8GB |
| Ubuntu 20.04     | GeForce RTX 3060 Laptop GPU |

If you have not completed the  [preparation process](README.md), it is necessary to complete it first.

## NVIDIA CUDA Drivers

First, it is necessary to download metadata information to allow the driver installation in your system by running the following command:

``` BASH
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
```

Then, manually is necessary to enter the [NVIDIA CUDA Developer](https://developer.nvidia.com/cuda-toolkit-archive) site with your login and password (if you don't have it, create one) and download the selected version of the CUDA Drivers for your card. e.g., For this guide, the *11.6.0* version was chosen.

``` BASH
sudo dpkg -i cuda-repo-ubuntu2004-11-6-local_11.6.0-510.39.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-6-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda nvidia-cuda-toolkit
```
### Test the NVIDIA CUDA Drivers

If the configuration is correct, the command `nvidia-smi` should show an output similar to this one:

<p align="center">
  <img src="https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/blob/main/config-nvidia-ubuntu/img/nvidia-smi-20-04.png?raw=true" alt="NVIDIA output command"/>
</p>


If this output is shown, is it possible to continue with the [Docker NVIDIA Support](https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/tree/main/config-nvidia-ubuntu/#docker-nvidia-support)

## CuDNN Drivers (optional)

If all the previous procedure is completed and the NVIDIA Developer web page session is active, it is possible to download the binary using the `wget`. e.g., for this example, version *8.3.1* was selected.

``` BASH
wget https://developer.nvidia.com/compute/cudnn/secure/8.3.1/local_installers/11.5/cudnn-local-repo-ubuntu2004-8.3.1.22_1.0-1_amd64.deb
```

If the `wget` command fails is necessary to enter manually to the [Official Documentation](https://developer.nvidia.com/rdp/cudnn-archive), choose the version again, and log in with your previously created credentials, download the package and install it.

``` BASH
sudo dpkg -i cudnn-local-repo-ubuntu2004-8.3.1.22_1.0-1_amd64.deb
```

### Test your CuDNN Compiler

If the process is doned correctly the `nvcc -V` command should output:

> nvcc: NVIDIA (R) Cuda compiler driver
> Copyright (c) 2005-2019 NVIDIA Corporation
> Built on Sun_Jul_28_19:07:16_PDT_2019
> Cuda compilation tools, release 10.1, V10.1.243

In addition is possible to compile classic CUDA examples using language C, more information in the [First CUDA Example](https://developer.nvidia.com/blog/easy-introduction-cuda-c-and-c/)
