FROM tensorflow/tensorflow:2.7.1-gpu
MAINTAINER Diego Rincon-Yanez <diego-rincon@outlook.com>

USER root

RUN adduser jupyter

USER jupyter

ENV PORT 8888

ENV PATH="${PATH}:/home/jupyter/.local/bin"

WORKDIR /home/jupyter

RUN python3 -m pip install --no-cache --upgrade setuptools pip

RUN pip install jupyter && pip install jupyterlab

EXPOSE $PORT

ENTRYPOINT ["jupyter", "lab","--ip=0.0.0.0","--allow-root"]
