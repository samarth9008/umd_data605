FROM ubuntu:20.04
ENV DEBIAN_FRONTEND noninteractive

# Fix old repo for ubuntu:20.04.
RUN mkdir /install
ADD fix_apt_install.sh /install/
RUN /install/fix_apt_install.sh

RUN apt-get -y update
RUN apt-get -y upgrade

# Install system utilities.
RUN apt install -y --no-install-recommends --fix-missing \
    sudo \
    curl \
    systemctl \
    gnupg \
    git \
    vim

# Install Python.
RUN apt install -y --no-install-recommends \
    python3 \
    python3-pip \
    python3-dev

RUN python3 --version
RUN pip3 --version

RUN python3 -m pip install --upgrade pip
    
# Install Python packages.
RUN pip3 install \
    ipython \
    jupyter \
    jupyter-contrib-core \
    jupyter-contrib-nbextensions \
    yapf

# Install MongoDB.
RUN curl -fsSL https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add - && \
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list && \
    apt update && \
    curl -O http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb && \
    dpkg -i ./libssl1.1_1.1.1f-1ubuntu2_amd64.deb && \
    apt install -y mongodb-org mongodb-org-server mongodb-org-shell

RUN pip3 install \
    pymongo \
    ipython-mongo

# Config.
ADD etc_sudoers /install/
COPY etc_sudoers /etc/sudoers
COPY bashrc /root/.bashrc

# Install Jupyter extensions.
ADD install_jupyter_extensions.sh /install
RUN /install/install_jupyter_extensions.sh

# Report package versions.
ADD version.sh /install/
RUN /install/version.sh 2>&1 | tee version.log

# Jupyter.
EXPOSE 8888
