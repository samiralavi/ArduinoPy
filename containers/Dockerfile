FROM ubuntu:22.04

ARG USERNAME=dev_user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# No WORKDIR should be set otherwise Github actions fail. 
# WORKDIR /home/$USERNAME

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && chown -R $USER_UID:$USER_GID /home/$USERNAME

# Install and configure packages required from ubuntu repository
RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" \
    apt-get install -y curl gnupg build-essential bash-completion nano \
    python3 python3-dev python3-pip git ca-certificates curl wget \
    python3-venv python-is-python3

# Downloads and installs the latest version of cmake
RUN wget https://github.com/Kitware/CMake/releases/download/v3.25.1/cmake-3.25.1-linux-x86_64.sh \
    && chmod +x cmake-3.25.1-linux-x86_64.sh \
    && ./cmake-3.25.1-linux-x86_64.sh --prefix=/usr/local --skip-license

USER $USERNAME

# Update pip
RUN pip3 install --upgrade pip

# installs the specified version of poetry
RUN curl -sSL https://install.python-poetry.org | python3 -
ENV PATH="/home/${USERNAME}/.local/bin:${PATH}"

USER root

# enables command completion on bash
RUN poetry completions bash > /etc/bash_completion.d/poetry
