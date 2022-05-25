# Prepare your Ubuntu 20.04 LTS

This guide will help you install and configure the NVIDIA CUDA drivers and the CuDNN binaries in a `Ubuntu 20.04` Linux machine to support your graphics card's compatibility and integrate it with your docker engine. 

This guide was tested on the following configurations and was updated up to the `TensorFlow version 2.7` and `NVIDIA CUDA Drivers version 11.6.0`; more changes and updates will be posted here.

| Operative System | Graphic Card Model |
| ---------------- | ------------------ |
| Ubuntu 20.04     | GeForce GTX 1060 6GB |
| Ubuntu 20.04     | GeForce RTX 3060 Laptop GPU |

## Preparation

First, it is mandatory to complete the docker [installation](https://docs.docker.com/engine/install/ubuntu/), then update and upgrade the system to install your machine drivers from the public repositories.

```
sudo apt-get update
sudo apt-get upgrade -y
sudo ubuntu-drivers autoinstall
```

Initially, check the output of the following command `lspci | grep -i nvidia` to see if your device is recognized by the operative system. e.g., The following output is the example for the *GeForce GTX 1060* card. If this command does not exit any input, check the physical connection of your device; otherwise, you cannot continue.

> 01:00.0 VGA compatible controller: NVIDIA Corporation GP106 [GeForce GTX 1060 6GB] (rev a1)
> 01:00.1 Audio device: NVIDIA Corporation GP106 High Definition Audio Controller (rev a1)

If your graphic card is different is **recommended** to check which version of [`CUDA Drivers`](https://www.nvidia.com/download/index.aspx) is compatible with your system.

**DISCLAIMER:** this is **not an official documentation** guide.

## NVIDIA CUDA Drivers

Once the preparation is finished, it is necessary to download metadata information to allow the driver installation in your system by running the following command:
tf@function
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
```

Then, manually is necessary to enter the [NVIDIA CUDA Developer](https://developer.nvidia.com/cuda-toolkit-archive) site with your login and password (if you don't have it, create one) and download the selected version of the CUDA Drivers for your card. e.g., For this guide, the *11.6.0* version was chosen.

```
sudo dpkg -i cuda-repo-ubuntu2004-11-6-local_11.6.0-510.39.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-6-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda nvidia-cuda-toolkit
```
### Test the NVIDIA CUDA Drivers

If the configuration is correct, the command `nvidia-smi` should show an output similar to this one:

> +-----------------------------------------------------------------------------+
> | NVIDIA-SMI 510.39.01    Driver Version: 510.39.01    CUDA Version: 11.6     |
> |-------------------------------+----------------------+----------------------+
> | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
> | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
> |                               |                      |               MIG M. |
> |===============================+======================+======================|
> |   0  NVIDIA GeForce ...  On   | 00000000:01:00.0 Off |                  N/A |
> | 42%   37C    P8     5W / 120W |     14MiB /  6144MiB |      0%      Default |
> |                               |                      |                  N/A |
> +-------------------------------+----------------------+----------------------+
>                                                                             
> +-----------------------------------------------------------------------------+
> | Processes:                                                                  |
> |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
> |        ID   ID                                                   Usage      |
> |=============================================================================|
> |    0   N/A  N/A      1168      G   /usr/lib/xorg/Xorg                  9MiB |
> |    0   N/A  N/A      1350      G   /usr/bin/gnome-shell                1MiB |
> +-----------------------------------------------------------------------------+

If this output is shown, is it possible to continue with the [Docker NVIDIA Support](#docker-nvidia-support)

## CuDNN Drivers (optional)

If all the previous procedure is completed and the NVIDIA Developer web page session is active, it is possible to download the binary using the `wget`. e.g., for this example, version *8.3.1* was selected.

```
wget https://developer.nvidia.com/compute/cudnn/secure/8.3.1/local_installers/11.5/cudnn-local-repo-ubuntu2004-8.3.1.22_1.0-1_amd64.deb
```

If the `wget` command fails is necessary to enter manually to the [Official Documentation](https://developer.nvidia.com/rdp/cudnn-archive), choose the version again, and log in with your previously created credentials, download the package and install it.

```
sudo dpkg -i cudnn-local-repo-ubuntu2004-8.3.1.22_1.0-1_amd64.deb
```

### Test your CuDNN Compiler

If the process is doned correctly the `nvcc -V` command should output:

> nvcc: NVIDIA (R) Cuda compiler driver
> Copyright (c) 2005-2019 NVIDIA Corporation
> Built on Sun_Jul_28_19:07:16_PDT_2019
> Cuda compilation tools, release 10.1, V10.1.243

In addition is possible to compile classic CUDA examples using language C, more information in the [First CUDA Example](https://developer.nvidia.com/blog/easy-introduction-cuda-c-and-c/)

## Docker NVIDIA Support

Finally, after the installation of the CUDA Drivers is necessary to enable the Docker engine to talk with the graphic card. If more information is needed, please check the [Official Tensorflow Docker Documentation](https://hub.docker.com/r/tensorflow/tensorflow/) or the [Github Repository](https://github.com/NVIDIA/nvidia-docker) for the connector.

The first step is to activate the Nvidia repository to the package system by executing:

```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
    && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
    && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

Execute an update of the package metadata and install the *nvidia-docker2* package. Finally, restart the docker engine service.

```
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

### Test your Docker Engine compatibility

Testing your docker engine interacting with the graphic card can be done by testing from the image perspective and from the image python environment. To check the operative system interaction, execute the command: 

```
sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
```

A similar output of the `nvidia-smi` command should appear.

> Unable to find image 'nvidia/cuda:11.0-base' locally
> 11.0-base: Pulling from nvidia/cuda
> 54ee1f796a1e: Pull complete 
> f7bfea53ad12: Pull complete 
> 46d371e02073: Pull complete 
> b66c17bbf772: Pull complete 
> 3642f1a6dfb3: Pull complete 
> e5ce55b8b4b9: Pull complete 
> 155bc0332b0a: Pull complete 
> Digest: sha256:774ca3d612de15213102c2dbbba55df44dc5cf9870ca2be6c6e9c627fa63d67a
> Status: Downloaded newer image for nvidia/cuda:11.0-base
>        
> +-----------------------------------------------------------------------------+
> | NVIDIA-SMI 510.39.01    Driver Version: 510.39.01    CUDA Version: 11.6     |
> |-------------------------------+----------------------+----------------------+
> | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
> | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
> |                               |                      |               MIG M. |
> |===============================+======================+======================|
> |   0  NVIDIA GeForce ...  On   | 00000000:01:00.0 Off |                  N/A |
> | 42%   37C    P8     5W / 120W |     14MiB /  6144MiB |      0%      Default |
> |                               |                      |                  N/A |
> +-------------------------------+----------------------+----------------------+
> 
> +-----------------------------------------------------------------------------+
> | Processes:                                                                  |
> |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
> |        ID   ID                                                   Usage      |
> |=============================================================================|
> |    0   N/A  N/A      1168      G                                       9MiB |
> |    0   N/A  N/A      1350      G                                       1MiB |
> +-----------------------------------------------------------------------------+

And to test your installation at a python level, execute: 

```
docker run --gpus all -it tensorflow/tensorflow:latest-gpu bash
```

This will download the latest GPU TensorFlow supported image and deliver a console.

> latest-gpu: Pulling from tensorflow/tensorflow
> d5fd17ec1767: Already exists 
> 50b37fabf1b7: Pull complete 
> 269c6117408b: Pull complete 
> 5ec4361a6d52: Pull complete 
> a490261931bd: Pull complete 
> fe780e40f0df: Pull complete 
> a7e8be5b40d6: Pull complete 
> c9c1c1b4a015: Pull complete 
> c8526746dd97: Pull complete 
> 66c7a05c493f: Pull complete 
> 8ae9c60fb8c8: Pull complete 
> 5ccc2202717d: Pull complete 
> 26ba94d72381: Pull complete 
> 4707b1097bdd: Pull complete 
> Digest: sha256:a34c2420739cd5a7b5662449bc21eb32d3d1c98063726ae2bd7db819cc93d72f
> Status: Downloaded newer image for tensorflow/tensorflow:latest-gpu
> 
> ________                               _______________                
> ___  __/__________________________________  ____/__  /________      __
> __  /  _  _ \_  __ \_  ___/  __ \_  ___/_  /_   __  /_  __ \_ | /| / /
> _  /   /  __/  / / /(__  )/ /_/ /  /   _  __/   _  / / /_/ /_ |/ |/ / 
> /_/    \___//_/ /_//____/ \____//_/    /_/      /_/  \____/____/|__/
> 
> 
> WARNING: You are running this container as root, which can cause new files in
> mounted volumes to be created as the root user on your host machine.
> 
> To avoid this, run the container by specifying your user's userid:
> 
> $ docker run -u $(id -u):$(id -g) args...
> 
> root@acd60364d554:/#

Once this shell is delivered, type the `python` command to enter the python interpreter and paste:

```
import tensorflow as tf
tf.config.list_physical_devices()
```

The output like this should appear, confirming that your docker engine supports the GPU processor type using the NVIDIA Drivers and is reachable from python.

> [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]

Happy hacking!! 🖖🖖.
