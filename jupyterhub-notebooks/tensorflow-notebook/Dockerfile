FROM jupyter/tensorflow-notebook:lab-3.1.9

LABEL maintainer="Mohit Sharma <mohitsharma44@gmail.com>"
ARG code_server_proxy_wheel="jupyter_codeserver_proxy-1.0-py3-none-any.whl"

# Fix DL4006
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
RUN apt-get update && apt-get install -yq --no-install-recommends \
    curl \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://code-server.dev/install.sh | sh && \
    rm -rf "${HOME}/.cache"

USER ${NB_UID}

ENV CODE_WORKINGDIR="${HOME}/work"

COPY ./dist/${code_server_proxy_wheel} ${HOME}/

RUN mamba install --quiet --yes \
    awscli=1.20.15 \
    boto3=1.18.15 \
    dask-core=2021.7.2 \
    dask=2021.7.2 \
    dask-gateway=0.9.0 \
    dask-kubernetes=2021.3.1 \
    dask-labextension=5.1.0 \
    dask-ml=1.9.0  \
    jupyter-resource-usage=0.6.0 \
    jupyter-server-proxy \
    jupyter-dash \
    plotly=5.2.1 \
    python-kaleido \
    nbgitpuller && \
    mamba clean --all -f -y

RUN jupyter labextension install @jupyterlab/server-proxy --no-build && \
    jupyter labextension install jupyterlab-plotly --no-build && \
    jupyter labextension install plotlywidget --no-build && \
    jupyter lab build && \
    jupyter lab clean -y && \
    npm cache clean --force

RUN pip install --quiet --no-cache-dir plotly-express==0.4.1 "${HOME}/${code_server_proxy_wheel}" && \
    rm "${HOME}/${code_server_proxy_wheel}" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

WORKDIR "${HOME}"
