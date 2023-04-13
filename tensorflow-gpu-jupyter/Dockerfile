FROM tensorflow/tensorflow:2.9.3-gpu

ARG BUILD_DATE
ENV PORT 8888

LABEL org.opencontainers.image.authors="rinconyanezd@gmail.com" \
	org.opencontainers.image.vendor="Diego, Rincon-Yanez" \
	org.opencontainers.image.title="Tensorflow GPU Jupyter" \
	org.opencontainers.image.created=$BUILD_DATE \
	org.opencontainers.image.version="2.9.3" \
	org.opencontainers.image.revision="1.0" \
	org.opencontainers.image.url="https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter" \
	org.opencontainers.image.documentation="https://github.com/d1egoprog/docker-tensorflow-gpu-jupyter" 

RUN apt-get -y update && apt-get -y install git

USER root

RUN adduser jupyter

USER jupyter

ENV PATH="${PATH}:/home/jupyter/.local/bin"

WORKDIR /home/jupyter

RUN mkdir /home/jupyter/data; mkdir /home/jupyter/notebooks

COPY ../tests/gpu_check.ipynb /home/jupyter/

COPY ../tests/env_preparation.ipynb /home/jupyter/ 

RUN python3 -m pip install --no-cache --upgrade setuptools pip

RUN pip install jupyter && pip install jupyterlab

EXPOSE $PORT

ENTRYPOINT ["jupyter", "lab","--ip=0.0.0.0","--allow-root"]
