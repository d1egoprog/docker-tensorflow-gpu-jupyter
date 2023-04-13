# TensorFlow GPU - Jupyter Lab/Notebook - Dockerized Alternative

This repository exposes and tracks a customized Docker Image for the TensorFlow package **with GPU support** and the Jupyter Lab or Notebook environments coexisting and ready to use. This custom build intends to be used on personal or small research teams or projects.

A ready-to-use [image](https://hub.docker.com/r/d1egoprog/tensorflow-gpu-jupyter) from Docker Hub is provided, along with the [deploy instructions](#deploy-alternatives) and the possibility of downloading and customizing the image through the Dockerfile using simple [build instructions](#custom-build). The assembled version uses the TensorFlow version `2.x` and `1.15` library version; however, it is possible to modify the base image in the `FROM` variable via the Dockerfile.

# Content

 1. [Pre-requisites](#pre-requisites)
 2. [Deploy Alternatives](#deploy-alternatives)
 3. [Custom Build](#custom-build)
 4. [Troublehsooting](#troublehsooting)

## Pre-requisites

In addition to the mandatory docker [installation](https://docs.docker.com/engine/install/ubuntu/), complying with the deployment prerequisites is necessary to activate the NVIDIA Driver successfully and obtain the output from the `nvidia-smi` command; if your environment is ready, you can skip the pre-requisites part.

Small guides are provided to set up the environment on a [Ubuntu](https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/tree/main/config-nvidia-ubuntu/) environment. 

This image was tested on these configurations:

| Operative System | Graphic Card Model |
| ---------------- | ------------------ |
| Ubuntu 20.04     | GeForce GTX 1060 |
| Ubuntu 20.04     | GeForce GTX 1070 |
| Ubuntu 20.04     | GeForce RTX 3060 Laptop GPU |
| Ubuntu 22.04     | GeForce RTX 3060 Laptop GPU |
| Windows 10 Enterprise     | QUADRO RTX 3000 |

If your graphic card is different is **recommended** to check which version of [`CUDA Drivers`](https://www.nvidia.com/download/index.aspx) is compatible with your system.

**DISCLAIMER:** this is **not an official documentation** guide.    

## Deploy Alternatives

Two options for deploying the prebuilt docker image are provided: the `docker-compose` tool and the `docker` command from the CLI utility.

### Deploy using Docker CLI

Directly run the `docker` command like the following example. e.g., changing two variables.

``` bash
docker run -v tfgpu_jl_data:/home/jupyter/data -v tfgpu_jl_ipynb:/home/jupyter/notebooks -p 8888:8888 -name sandbox_tfgpu_jl --gpus all d1egoprog/tensorflow-gpu-jupyter:2.9.3-lab
```

### Deploy using docker-compose

Download the prepared `docker-compose.yaml` file from the repository via `wget` and execute the command using the utility or copy the content manually from this repository.

``` bash
wget https://raw.githubusercontent.com/d1egoprog/tensorflow-gpu-jupyter-docker/main/compose.yaml
docker-compose -p sandbox up -d
```
The option `-p` creates a new stack called *sandbox*, just for these instructions.

### Backups

The provided alternatives create two extra volumes to maintain a backup from the datasets `tfgpu_jl_data` and another to preserve the actual notebooks `tfgpu_jl_ipynb`. The only consideration is to keep the notebooks stored in the pre-created folder `notebook` and the data used in the notebooks in the pre-created `data` folder.

### Testing the Installation

To check the functionality, you can open a web browser window to your docker-engine `IP` and the chosen service, e.g., `PORT=8888`; if you run this on your machine should be on [localhost:8888/lab](http://localhost:8888/lab) and [localhost:9988/lab](http://localhost:9988/lab) if was deployed using compose. After that, the Jupyter Lab landing page should deploy if the deployment went correctly, asking for the session token. To obtain the token, just query the system log by using the command:

``` bash
docker logs sandbox_tfgpu_jl
```

An output similar to this one should appear:

>     To access the server, open this file in a browser:
>         file:///home/jupyter/.local/share/jupyter/runtime/jpserver-1-open.html
>     Or copy and paste one of these URLs:
>         http://3538c43d20f3:8888/lab?token=<TOKEN>
>      or http://127.0.0.1:8888/lab?token=<TOKEN>

Take the value of the `token` variable from the URL, *<TOKEN>* in this example, and paste it into the token textbox displayed in the browser.

Happy hacking!! 🖖🖖.

## Custom Build

If is necessary is possible edit the image to add additional parameters. To build the image locally, clone the repository:

``` bash
git clone https://github.com/d1egoprog/tensorflow-gpu-jupyter-docker.git
```
And follow the build alternatives.

### Build using Docker CLI

Edit the Dockerfile using your favorite text editor and use the `docker` command CLI tool to build the image:

``` bash
docker build -t sandbox_tfgpu_jl tensorflow-gpu-jupyter/.
```

To run the image, execute via ['docker'](#deploy-using-docker-cli) command.

### Build using docker-compose

If preferred, a docker-compose file type is also available with the standard build from the `Dockerfile`. To run the service, run the command:

``` bash
git clone https://github.com/d1egoprog/tensorflow-gpu-jupyter-docker.git
cd tensorflow-gpu-jupyter-docker/
```

Edit the Dockerfile using your favorite text editor and run the command to launch the configuration using the file `build.yaml`:

```
docker-compose -p sandbox -f build.yaml up -d
```

## Troublehshooting

If you have any questions in deployment or build, and any error is found, please open an issue. And contributing is always welcome. The [Github repository URL](https://github.com/d1egoprog/tensorflow-gpu-jupyter-docker).
