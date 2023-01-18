# Prepare your Environment

These guides will help you install and configure the NVIDIA CUDA drivers in a Ubuntu Linux machine to support your graphics card's compatibility and integrate it with your docker engine. These were tested on the following environments.

| Operative System | Graphic Card Model |
| ---------------- | ------------------ |
| Ubuntu 20.04     | GeForce GTX 1060 6GB |
| Ubuntu 20.04     | GeForce GTX 1070 8GB |
| Ubuntu 20.04     | GeForce RTX 3060 Laptop GPU |
| Ubuntu 22.04     | GeForce RTX 3060 Laptop GPU |

**DISCLAIMER:** this are **not an official documentation** guides.

## Preparation

First, it is mandatory to complete the docker [installation](https://docs.docker.com/engine/install/ubuntu/), then update and upgrade the system to install your machine drivers from the public repositories.

``` BASH
sudo apt-get update
sudo apt-get upgrade -y
sudo ubuntu-drivers autoinstall
```

Initially, check the output of the following command `lspci | grep -i nvidia` to see if your device is recognized by the operative system. e.g., The following output is the example for the *GeForce GTX 1060* card. If this command does not exit any input, check the physical connection of your device; otherwise, you cannot continue.

> 01:00.0 VGA compatible controller: NVIDIA Corporation GP106 [GeForce GTX 1060 6GB] (rev a1)
>
> 01:00.1 Audio device: NVIDIA Corporation GP106 High Definition Audio Controller (rev a1)

If your graphic card is different is **recommended** to check which version of [`CUDA Drivers`](https://www.nvidia.com/download/index.aspx) is compatible with your system.

## Docker NVIDIA Support

Finally, after the installation of the CUDA Drivers is necessary to enable the Docker engine to talk with the graphic card. If more information is needed, please check the [Official Tensorflow Docker Documentation](https://hub.docker.com/r/tensorflow/tensorflow/) or the [Github Repository](https://github.com/NVIDIA/nvidia-docker) for the connector.

The first step is to activate the Nvidia repository to the package system by executing:

``` BASH
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
    && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
    && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

Execute an update of the package metadata and install the *nvidia-docker2* package. Finally, restart the docker engine service.

``` BASH
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

### Test your Docker Engine compatibility

Testing your docker engine interacting with the graphic card can be done from the image perspective and the image python environment. To check the operative system interaction, execute the command: 

``` BASH
sudo docker run --rm --gpus all nvidia/cuda:12.0.0-base-ubuntu20.04 nvidia-smi
```

A similar output of the `nvidia-smi` command should appear.

<p align="center">
  <img src="https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/blob/main/config-nvidia-ubuntu/img/docker-nvidia-smi.png?raw=true" alt="NVIDIA output command"/>
</p>



And to test your installation at a python level, execute: 

```
sudo docker run --gpus all -it tensorflow/tensorflow:latest-gpu bash
```

This will download the latest GPU TensorFlow supported image and deliver a console.

<p align="center">
  <img src="https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/blob/main/config-nvidia-ubuntu/img/docker-tensorflow.png?raw=true" alt="NVIDIA output command"/>
</p>


Once this shell is delivered, type the `python` command to enter the python interpreter and paste:

``` PYTHON
import tensorflow as tf
tf.config.list_physical_devices()
```

The output like this should appear, confirming that your docker engine supports the GPU processor type using the NVIDIA Drivers and is reachable from python.

> [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]

Happy hacking!! 🖖🖖.



