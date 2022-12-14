
# 開発用ユーザ追加

# Ubuntu 22.04 (jammy)
# https://hub.docker.com/_/ubuntu/tags?page=1&name=jammy
ARG ROOT_CONTAINER=ubuntu:22.04

FROM $ROOT_CONTAINER

ARG UBNT_USER="devuser01"
ARG UBNT_UID="1000"
ARG UBNT_GID="100"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update --yes && \
    apt-get upgrade --yes && \
    apt-get install --yes --no-install-recommends \
    bzip2 \
    ca-certificates \
    locales \
    sudo \
    tini \
    wget \
    vim  \
    zip &&\
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

ENV SHELL=/bin/bash \
    UBNT_USER="${UBNT_USER}" \
    UBNT_UID=${UBNT_UID} \
    UBNT_GID=${UBNT_GID} \
    LC_ALL=ja_JP.UTF-8 \
    LANG=ja_JP.UTF-8 \
    LANGUAGE=ja_JP.UTF-8 \
    HOME="/home/${UBNT_USER}"

RUN sed -i 's/^#force_color_prompt=yes/force_color_prompt=yes/' /etc/skel/.bashrc
RUN useradd -l -m -s /bin/bash -N -u ${UBNT_UID} ${UBNT_USER} && \
    usermod -aG sudo ${UBNT_USER} && \
    echo "${UBNT_USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER ${UBNT_USER}
WORKDIR ${HOME}
RUN cat /etc/skel/.bashrc > ./.bashrc

