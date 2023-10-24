# syntax=docker/dockerfile:latest
FROM ubuntu:22.04

LABEL maintainer="CANFAR Project <support@canfar.net>"

USER root

WORKDIR /tmp

RUN set -ex \
    && apt-get update --yes --quiet --fix-missing \
    && apt-get install --no-install-recommends --yes --quiet acl \
        bzip2 \
        curl \
        git \
        locales \
        nano \
        rsync \
        openssh-client \
        sudo \
        vim \
        wget \
        zsh \
        tcsh \
    && apt-get clean --yes \
    && apt-get autoremove --purge --quiet --yes \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/tmp/* \
    && rm -rf /tmp/* \
    && rm -rf /usr/share/man \
    && rm -rf /usr/share/doc \
    && rm -rf /usr/share/doc-base \
    && rm -rf ~/.cache 

ADD nsswitch.conf /etc/

# install micromamba and initialize
ARG PYTHON_VERSION

ENV CONDA_DIR=/opt/conda
ENV PATH="${CONDA_DIR}/bin:${PATH}"

COPY condarc "${CONDA_DIR}/.condarc"

RUN set -x && \
    curl -Ls https://micromamba.snakepit.net/api/micromamba/linux-64/latest | tar -xvj bin/micromamba && \
    ./bin/micromamba install \
        --root-prefix="${CONDA_DIR}" \
        --prefix="${CONDA_DIR}" \
        --yes \
        python=${PYTHON_VERSION}

# conda profile scripts by default
RUN ln -s ${CONDA_DIR}/etc/profile.d/conda.sh /etc/profile.d/conda.sh

# # install minimimal base conda environment
# # RUN conda update
# RUN micromamba install \
#     conda \
#     pip \
#     conda-libmamba-solver \
#     pip-tools \
#     && conda clean --all --quiet --force --yes 