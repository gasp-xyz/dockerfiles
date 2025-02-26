ARG RUST_VERSION=1.77

FROM rust:${RUST_VERSION}-bookworm

# metadata
LABEL summary="Image for Rust and WASM compilation." \
	name="gaspxyz/rust-builder" \
	maintainer="support@gasp.xyz" \
	version="1.0" \
	description="libssl-dev, clang, libclang-dev, lld, cmake, make, git, pkg-config \
curl, time, jq, lsof, rhash, rust stable, rust nightly, sccache" \
	io.parity.image.vendor="Gasp" \
	io.parity.image.source="https://github.com/gasp-xyz/dockerfiles/blob/main/\
rust-builder/Dockerfile" \
	io.parity.image.documentation="https://github.com/gasp-xyz/dockerfiles/blob/main/\
rust-builder/Dockerfile" 

WORKDIR /builds

# install tools and dependencies
RUN set -eux && \
		apt-get -y update; \
		apt-get install -y --no-install-recommends \
		libssl-dev make cmake graphviz clang libclang-dev llvm \
		git pkg-config curl time rhash ca-certificates zstd \
		python3 python3-pip lsof ruby ruby-bundler git-restore-mtime xz-utils unzip gnupg protobuf-compiler && \
		# Install mold linker
		curl -L https://github.com/rui314/mold/releases/download/v2.36.0/mold-2.36.0-$(uname -m)-linux.tar.gz \
		| tar -C /usr/local --strip-components=1 -xzf - && ln -sf /usr/local/bin/mold "$(realpath /usr/bin/ld)" && \
		# Install sccache
		curl -L https://github.com/mozilla/sccache/releases/download/v0.10.0/sccache-v0.10.0-$(uname -m)-unknown-linux-musl.tar.gz -O && \
		tar -C /usr/local/bin --strip-components=1 --wildcards "*sccache" -xzf sccache-v0.10.0-$(uname -m)-unknown-linux-musl.tar.gz && \
		chmod +x /usr/local/bin/sccache && rm -rf sccache-v0.10.0-$(uname -m)-unknown-linux-musl.tar.gz && \
		# Install Nodejs
		curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && apt-get install -y nodejs && \
		# Install Docker tools
		curl -fsSL https://get.docker.com | sh && \
		# apt clean up
		apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

ARG RUST_TOOLCHAIN=nightly-2024-01-20

RUN rustup install $RUST_TOOLCHAIN && rustup default $RUST_TOOLCHAIN && \
	rustup target add wasm32-unknown-unknown && \
	rustup component add rust-src rustfmt clippy && \
	# cargo install --git https://github.com/paritytech/try-runtime-cli --tag v0.3.3 --locked && \
	# cargo clean up
	# removes compilation artifacts cargo install creates (>250M)
	rm -rf "${CARGO_HOME}/registry" "${CARGO_HOME}/git"


# Install Google Cloud CLI
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - && \
    apt-get update -y && \
    apt-get install -y \
        google-cloud-cli \
        google-cloud-cli-gke-gcloud-auth-plugin \
        kubectl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    gcloud config set core/disable_usage_reporting true && \
    gcloud config set component_manager/disable_update_check true && \
    gcloud config set metrics/environment github_docker_image && \
    gcloud --version

# Install moonrepo `proto` binary
RUN curl -fsSL https://moonrepo.dev/install/proto.sh -o /tmp/proto_install.sh && \
    chmod +x /tmp/proto_install.sh && \
    bash /tmp/proto_install.sh 0.47.1 --yes && \
    rm /tmp/proto_install.sh
	
# # cache handler
# ENV	RUSTC_WRAPPER=sccache \
# # show backtraces
#   	RUST_BACKTRACE=1