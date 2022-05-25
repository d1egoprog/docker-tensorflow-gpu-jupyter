# TensorFlow GPU - Jupyter Lab/Notebook - Dockerized Alternative

This repository is dedicated to expose and track a customized Docker Image for the TensorFlow package **with GPU support** and the Jupyter Lab or Notebook environments coexisting and ready to use. This custom build intends to be used on personal or small research teams or projects.

A ready-to-use [image](https://hub.docker.com/r/d1egoprog/tensorflow-gpu-jupyter) from Docker Hub is provided, along with the [deploy instructions](#deploy-alternatives) and the possibility of downloading and customizing the image through the Dockerfile using simple [build instructions](#build-alternatives). The assembled version uses the 2.7 library version; however, it is possible to modify the base image in the `FROM` variable via the Dockerfile.

## Deploy Alternatives

To deploy the prebuilt docker image, two options are provided: use the `docker-compose` tool and the `docker` command from the CLI utility. In addition to the mandatory docker [installation](https://docs.docker.com/engine/install/ubuntu/), complying with the deployment prerequisites is necessary to activate the NVIDIA Driver successfully obtain the output from the `nvidia-smi` command; if your environment is ready, you can skip the pre-requisites part.

### Pre-requisites

A small guide is provided to set up the environment on a [Ubuntu 20.04](https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter/config-nvidia-ubuntu-20.04/). This guide was tested on the following configurations and was updated up to the `TensorFlow version 2.7` and `NVIDIA CUDA Drivers version 11.6.0`, more changes and updates will be posted here.

| Operative System | Graphic Card Model |
| ---------------- | ------------------ |
| Ubuntu 20.04     | GeForce GTX 1060 6GB |
| Ubuntu 20.04     | GeForce RTX 3060 Laptop GPU |

If your graphic card is different is **recommended** to check which version of [`CUDA Drivers`](https://www.nvidia.com/download/index.aspx) is compatible with your system.

**DISCLAIMER:** this is **not an official documentation** guide.    

### Backups

The provided alternatives create two extra volumes to maintain a backup from the datasets `tfgpu_jupyter_data` and another to preserve the actual notebooks `tfgpu_jupyter_ipynb`. The only consideration is to keep the notebooks stored in the pre-created folder `notebook` and the data used in the notebooks in the pre-created `data` folder.

### Deploy using docker-compose

Download the prepared `deploy.yml` file from the repository via `wget` and execute the command using the utility or copy the content manually from this repository.

```
wget https://raw.githubusercontent.com/d1egoprog/tensorflow-gpu-jupyter-docker/main/deploy.yml
docker-compose -p sandbox -f deploy.yml up -d
```

The option `-p` creates a new stack called *sandbox*, just for these instructions.

### Deploy using Docker CLI

Directly run the `docker` command like the following example. e.g., changing two variables.

```
docker run -v tfgpu_jupyter_data:/home/jupyter/data -v tfgpu_jupyter_ipynb:/home/jupyter/notebooks -p 8888:8888 -name sandbox_tfgpu_jupyter --gpus all d1egoprog/tensorflow-gpu-jupyter:2.7-lab
```

### Testing the Installation

To check the functionality, you can open a web browser window to your docker-engine `IP` and the chosen service, e.g., `PORT=8888`; if you run this on your machine should be on [localhost:8888/lab](http://localhost:8888/lab). The Jupyter Lab landing page should deploy if the deployment went correctly, asking for the session token. To obtain the token, just query the system log by using the command:

```
docker logs sandbox_tfgpu_jupyter
```

An output similar to this one should appear:

> [I 2022-05-25 09:15:39.319 ServerApp] jupyterlab | extension was successfully linked.
> [I 2022-05-25 09:15:39.328 ServerApp] nbclassic | extension was successfully linked.
> [I 2022-05-25 09:15:39.329 ServerApp] Writing Jupyter server cookie secret to /home/jupyter/.local/share/jupyter/runtime/jupyter_cookie_secret
> [I 2022-05-25 09:15:39.482 ServerApp] notebook_shim | extension was successfully linked.
> [I 2022-05-25 09:15:39.503 ServerApp] notebook_shim | extension was successfully loaded.
> [I 2022-05-25 09:15:39.504 LabApp] JupyterLab extension loaded from /home/jupyter/.local/lib/python3.8/site-packages/jupyterlab
> [I 2022-05-25 09:15:39.504 LabApp] JupyterLab application directory is /home/jupyter/.local/share/jupyter/lab
> [I 2022-05-25 09:15:39.506 ServerApp] jupyterlab | extension was successfully loaded.
> [I 2022-05-25 09:15:39.509 ServerApp] nbclassic | extension was successfully loaded.
> [I 2022-05-25 09:15:39.509 ServerApp] Serving notebooks from local directory: /home/jupyter
> [I 2022-05-25 09:15:39.509 ServerApp] Jupyter Server 1.17.0 is running at:
> [I 2022-05-25 09:15:39.509 ServerApp] http://3538c43d20f3:8888/lab?token=<TOKEN>
> [I 2022-05-25 09:15:39.509 ServerApp]  or http://127.0.0.1:8888/lab?token=<TOKEN>
> [I 2022-05-25 09:15:39.509 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
> [W 2022-05-25 09:15:39.512 ServerApp] No web browser found: could not locate runnable browser.
> [C 2022-05-25 09:15:39.512 ServerApp] 
>     
>     To access the server, open this file in a browser:
>         file:///home/jupyter/.local/share/jupyter/runtime/jpserver-1-open.html
>     Or copy and paste one of these URLs:
>         http://3538c43d20f3:8888/lab?token=<TOKEN>
>      or http://127.0.0.1:8888/lab?token=<TOKEN>
> [I 2022-05-25 09:16:01.690 LabApp] 302 GET /lab (172.25.0.1) 1.16ms

Take the value of the `token` variable from the URL, *<TOCKEN>* in this example, and paste it into the token textbox displayed in the browser.

Happy hacking!! 🖖🖖.

## Build Alternatives

If is necessary is possible to 

### Build using Docker CLI

To build the image locally, clone the repository: 

```
git clone https://github.com/d1egoprog/tensorflow-gpu-jupyter-docker.git
```

Edit the Dockerfile using your favorite text editor and use the `docker` command CLI tool to build the image:

```
docker build -t sandbox_tfgpu_jupyter tensorflow-gpu-jupyter/.
```

To run the image, execute via ['docker'](#deploy-using-docker-cli) command.

### Build using docker-compose

If preferred, a docker-compose file is also available with the standard build from the `Dockerfile`. To run the service, run the command:

```
git clone https://github.com/d1egoprog/tensorflow-gpu-jupyter-docker.git
cd tensorflow-gpu-jupyter-docker/
```

Or use the compose file to build the Docker image, storing the following into a new `build.yml` file. 

```
version: '3.7'

volumes:
  tfgpu_jupyter_ipynb:
  tfgpu_jupyter_data:

services:
  tfgpu_jupyter:
    build: tensorflow-gpu-jupyter/
    ports:
      - "8888:8888"
    restart: always
    volumes:
      - tfgpu_jupyter_ipynb:/home/jupyter/notebooks
      - tfgpu_jupyter_data:/home/jupyter/data
    deploy:
      resources:
        reservations:
          devices:
          - capabilities: [gpu]
```

Edit the Dockerfile using your favorite text editor and run the command `docker-compose -p sandbox -f build.yml up -d`.

## Contact

If you have any questions in deployment or build and any error is found, please contact me by opening an issue. And contributing is always welcome. The [Github repository URL](https://github.com/d1egoprog/tensorflow-gpu-jupyter-docker).

