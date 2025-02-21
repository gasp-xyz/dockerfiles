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
    binfmt-support \
    protobuf-compiler

# Install Zig (Direct Download - Multi-Platform)
ARG ZIG_VERSION=0.13.0
ARG TARGETPLATFORM
RUN case ${TARGETPLATFORM} in \
        linux/amd64)  ZIG_ARCH=x86_64   ;; \
        linux/arm64)  ZIG_ARCH=aarch64  ;; \
        *) echo "Unsupported platform: ${TARGETPLATFORM}"; exit 1 ;; \
    esac && \
    curl -o zig.tar.xz "https://ziglang.org/download/${ZIG_VERSION}/zig-linux-${ZIG_ARCH}-${ZIG_VERSION}.tar.xz" && \
    tar -xf zig.tar.xz && \
    sudo mv "zig-linux-${ZIG_ARCH}-${ZIG_VERSION}" /usr/local/zig && \
    sudo ln -s /usr/local/zig/zig /usr/local/bin/zig

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

# Install Google Cloud CLI
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - && \
    sudo apt-get update -y && \
    sudo apt-get install -y google-cloud-cli google-cloud-cli-gke-gcloud-auth-plugin
    
# Clean up
RUN sudo apt-get clean && \
    sudo rm -rf /var/lib/apt/lists/*

# https://github.com/sfackler/rust-openssl/issues/2333
RUN sudo ln -s /usr/include/x86_64-linux-gnu/openssl/opensslconf.h /usr/include/openssl/opensslconf.h && \
    sudo ln -s /usr/include/x86_64-linux-gnu/openssl/configuration.h /usr/include/openssl/configuration.h
