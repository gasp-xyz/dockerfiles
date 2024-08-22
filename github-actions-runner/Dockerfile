FROM ghcr.io/actions/actions-runner:latest

# Install QEMU and other necessary packages
RUN sudo apt update -y && \
    sudo apt install -y \
    curl \
    git \
    unzip \
    build-essential \
    pkg-config \
    libssl-dev \
    libudev-dev \
    libclang-dev \
    clang \
    zstd \
    qemu-user-static \
    binfmt-support

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - && \
    sudo apt-get install -y nodejs

# Install Yarn
RUN sudo curl -fsSL --create-dirs -o /usr/local/bin/yarn https://github.com/yarnpkg/yarn/releases/download/v1.22.19/yarn-1.22.19.js && \
    sudo chmod +x /usr/local/bin/yarn

# Install Docker Compose
RUN sudo curl -SL https://github.com/docker/compose/releases/download/v2.24.7/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose && \
    sudo chmod +x /usr/local/bin/docker-compose

# Set up binfmt-support for QEMU
RUN sudo update-binfmts --enable qemu-arm
RUN sudo update-binfmts --enable qemu-aarch64

# Install additional tools for cross-compilation (optional, based on your needs)
RUN sudo apt install -y \
    gcc-aarch64-linux-gnu \
    g++-aarch64-linux-gnu \
    gcc-arm-linux-gnueabihf \
    g++-arm-linux-gnueabihf

# Clean up
RUN sudo apt-get clean && \
    sudo rm -rf /var/lib/apt/lists/*
