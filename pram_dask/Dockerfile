FROM daskdev/dask:2021.11.0-py3.9

RUN apt-get update --allow-releaseinfo-change -y
RUN apt-get upgrade --fix-missing -y
RUN apt-get install --no-install-recommends git build-essential python3-dev libpq-dev -y --fix-missing && \
    pip install -U pip

WORKDIR /src/

RUN cd /src/ && git clone -b dev-k https://github.com/quanted/pram_app.git
RUN cd /src/ && git clone -b dev-k --recursive https://github.com/quanted/pram_flask.git
#RUN cd /src && git clone -b dev-k https://github.com/quanted/pram_flask.git
RUN cd /src/pram_flask/ubertool && git submodule update --init && git submodule foreach --recursive git branch dev origin/dev && git submodule foreach --recursive git pull origin dev && git submodule foreach --recursive checkout origin dev
#RUN cd /src/pram_flask && git submodule update --init && git submodule foreach --recursive git checkout dev && git submodule foreach --recursive git pull origin dev

RUN conda create --name pyenv python=3.9

RUN conda run -n pyenv --no-capture-output pip install -r /src/pram_app/requirements.txt
RUN conda run -n pyenv --no-capture-output python --version

ENV PATH "/src:/src/pram_app:/src/pram_flask:${PATH}"
ENV PYTHONPATH "/src:/src/pram_app:/src/pram_flask:${PYTHONPATH}"

CMD ["conda", "run", "-n", "pyenv", "--no-capture-output", "dask-worker", "dask-scheduler:8786"]
