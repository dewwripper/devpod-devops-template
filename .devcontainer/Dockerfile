# Dockerfile
# Use Ubuntu 24.04 as the base image
FROM ubuntu:24.04

ARG DockerUSER
ARG DockerUID
ARG DockerGID

ENV DEBIAN_FRONTEND=noninteractive
ENV HOMEBREW_PREFIX="/home/linuxbrew/.linuxbrew"
ENV PATH="${HOMEBREW_PREFIX}/bin:${HOMEBREW_PREFIX}/sbin:${PATH}"
ENV DockerUSER=${DockerUSER}
ENV DockerUID=${DockerUID}
ENV DockerGID=${DockerGID}

# Update and install required packages
RUN apt-get update && apt-get install -y \
    curl \
    apt-transport-https \
    ca-certificates \
    lsb-release \
    gnupg \
    git \
    sudo \
    bash-completion \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

USER root

# Create non-root user
RUN if getent passwd $DockerUID > /dev/null; then userdel -rf $(getent passwd $DockerUID | cut -d ':' -f 1); fi \
    && groupadd --force --gid $DockerGID $DockerUSER \
    && useradd --no-log-init --uid $DockerUID --gid $DockerGID -m $DockerUSER \
    && mkdir -p /etc/sudoers.d/ \
    && echo $DockerUSER ALL=\(root\) NOPASSWD:ALL > /etc/sudoers`.d/$DockerUSER \
    && chmod 0440 /etc/sudoers.d/$DockerUSER

USER $DockerUSER

# Install Homebrew
RUN echo "Installing Homebrew..." && \
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" && \
    echo 'eval "$($HOMEBREW_PREFIX/bin/brew shellenv)"' >> \
    /home/${DockerUSER}/.bashrc

WORKDIR /home/$DockerUSER/

CMD [ "bash" ]