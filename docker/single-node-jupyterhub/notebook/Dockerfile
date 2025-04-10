# Image name: snj-base-notebook
FROM ubuntu:20.04
COPY custom.js /tmp/
WORKDIR /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get upgrade -y --no-install-recommends && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git=1:2.25.1-1ubuntu3 software-properties-common=0.99.9.12 wget fuse=2.9.9-3 \
    build-essential=12.8ubuntu1.1 python3-dev=3.8.2-0ubuntu2 python3-pip libargon2-dev=0~20171227-0.2 \
    libffi-dev=3.3-4 libpixman-1-dev=0.38.4-0ubuntu2.1 libcairo2-dev libpangox-1.0-dev=0.0.2-5ubuntu1 \
    libjpeg-dev=8c-2ubuntu8 libczmq-dev=4.2.0-2 curl=7.68.0-1ubuntu2.22 libcurl4-openssl-dev \
    libssl-dev dirmngr=2.2.19-3ubuntu2.2 gpg-agent=2.2.19-3ubuntu2.2 \
    && apt-get clean && rm -rf /var/lib/apt/lists/* && \
    # Make dodasts folder
    mkdir -p /usr/local/share/dodasts && \
    # Make script folder
    mkdir -p /usr/local/share/dodasts/script && \
    # Make bin folder
    mkdir -p /usr/local/share/dodasts/bin && \
    python3 -m pip install -U --no-cache-dir pip==23.0.1 setuptools==67.6.0 wheel==0.40.0 && \
    # Install sts-wire
    curl -L https://repo.cloud.cnaf.infn.it/repository/sts-wire/sts-wire-linux/2.1.5/sts-wire-linux-2.1.5 -o /usr/local/share/dodasts/bin/sts-wire && \
    chmod +x /usr/local/share/dodasts/bin/sts-wire && \
    ln -s /usr/local/share/dodasts/bin/sts-wire /usr/local/bin/sts-wire && \
    # Install oidc-agent
    apt-key adv --keyserver hkp://pgp.surfnet.nl --recv-keys ACDFB08FDC962044D87FF00B512839863D487A87 && \
    add-apt-repository "deb https://repo.data.kit.edu/ubuntu/20.04 ./" && \
    DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    liboidc-agent4 oidc-agent-cli oidc-agent-desktop oidc-agent && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    # Install jupyterlab
    python3 -m pip install --no-cache-dir \
    jupyterlab==3.2.3 \
    ipywidgets==8.0.4 \
    jedi-language-server==0.34.8 \
    notebook==6.4.5 \
    nbdime==3.1.1 \
    pycurl==7.45.2 \
    idna==2.8 \
    importlib-metadata==6.0.0 \
    ipympl==0.9.3 && \
    curl -fsSL https://deb.nodesource.com/setup_16.x | bash - && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends nodejs=16.19.1-1nodesource1 && \
    python3 -m pip install --no-cache-dir jupyterhub==1.5.0 && \
    npm install -g configurable-http-proxy@4.5.4 && \
    # Add custom js for collaborative button in notebooks
    cp /tmp/custom.js "$(python3 -c "import site; print(site.getsitepackages()[0])")/notebook/static/custom/" \
    && rm /tmp/custom.js
WORKDIR /jupyter-workspace
