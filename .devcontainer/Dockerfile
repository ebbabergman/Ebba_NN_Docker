FROM tensorflow/tensorflow:latest-py3
LABEL maintainer="ebba.bergman@farmbio.uu.se"

# Avoid warnings by switching to noninteractive
# ENV DEBIAN_FRONTEND=noninteractive
# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# install build utilities
RUN  apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y git && \
    libjpeg62 libjpeg62-dev && \
    sudo &&\
    rm -rf /var/lib/apt/lists/*

# check our python environment
RUN python3 --version
RUN pip3 --version

# Installing python dependencies
COPY .devcontainer/requirements.txt /tmp/pip-tmp/
RUN  pip --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp \
    #
    ## Create a non-root user to use if preferred
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # Add sudo support for the non-root user
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog