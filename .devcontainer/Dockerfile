FROM python:3.11

LABEL maintainer="Supphachoke Suntiwichaya <mrchoke@gmail.com>"

ENV TZ=Asia/Bangkok

# add fish repositories

RUN echo 'deb https://download.opensuse.org/repositories/shells:/fish:/release:/3/Debian_12/ /' >/etc/apt/sources.list.d/shells🐟release:3.list \
  && curl -fsSL 'https://download.opensuse.org/repositories/shells:/fish:/release:/3/Debian_12/Release.key' | gpg --dearmor >/etc/apt/trusted.gpg.d/shells_fish_release_3.gpg

# install debian packages

RUN apt update \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt install -y \
  curl \
  fish \
  locales \
  rsync \
  sudo \
  tmux \
  vim \
  && apt-get clean autoclean \
  && apt-get autoremove --yes \
  && rm -rf /var/lib/{apt,dpkg,cache,log}/

# change root shell

RUN chsh -s /usr/bin/fish root

# install locales

RUN sed --in-place '/en_US.UTF-8/s/^#//' /etc/locale.gen  \
  &&  sed --in-place '/th_TH.UTF-8/s/^#//' /etc/locale.gen \
  && locale-gen

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# set timezone

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# install python modules

RUN pip install --no-cache-dir -U pip \
  && pip install --no-cache-dir -U bandit autopep8 fastapi[all]

ARG USERNAME=fastapi
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

RUN echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME}

RUN groupadd --gid ${USER_GID} ${USERNAME}  \
  && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} -s /usr/bin/fish

USER ${USERNAME}

WORKDIR /workspace/fastapi

ENV PYTHONPATH=/workspace/fastapi

EXPOSE 8000