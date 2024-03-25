FROM rust:1.77-buster

# metadata
LABEL summary="Image for Rust and WASM compilation." \
	name="mangatasolutions/node-builder" \
	maintainer="admin@mangata.finance" \
	version="1.0" \
	description="libssl-dev, clang, libclang-dev, lld, cmake, make, git, pkg-config \
curl, time, jq, lsof, rhash, rust stable, rust nightly, sccache" \
	io.parity.image.vendor="Mangata Finance" \
	io.parity.image.source="https://github.com/mangata-finance/dockerfiles/blob/main/\
node-builder/Dockerfile" \
	io.parity.image.documentation="https://github.com/mangata-finance/dockerfiles/blob/main/\
node-builder/Dockerfile" 

WORKDIR /builds

# install tools and dependencies
RUN set -eux && \
		apt-get -y update; \
		apt-get install -y --no-install-recommends \
		libssl-dev make cmake graphviz clang libclang-dev llvm \
		git pkg-config curl time rhash ca-certificates zstd \
		python3 python3-pip lsof ruby ruby-bundler git-restore-mtime xz-utils unzip gnupg protobuf-compiler && \
		# Install mold linker
		curl -L https://github.com/rui314/mold/releases/download/v1.7.0/mold-1.7.0-$(uname -m)-linux.tar.gz \
		| tar -C /usr/local --strip-components=1 -xzf - && ln -sf /usr/local/bin/mold "$(realpath /usr/bin/ld)" && \
		# Install sccache
		curl -L https://github.com/mozilla/sccache/releases/download/v0.3.1/sccache-v0.3.1-$(uname -m)-unknown-linux-musl.tar.gz -O && \
		tar -C /usr/local/bin --strip-components=1 --wildcards "*sccache" -xzf sccache-v0.3.1-$(uname -m)-unknown-linux-musl.tar.gz && \
		chmod +x /usr/local/bin/sccache && rm -rf sccache-v0.3.1-$(uname -m)-unknown-linux-musl.tar.gz && \
		# Install Nodejs
		curl -fsSL https://deb.nodesource.com/setup_16.x | bash - && apt-get install -y nodejs && \
		# Install Docker tools
		curl -fsSL https://get.docker.com | sh && \
		# apt clean up
		apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

ARG RUST_TOOLCHAIN=nightly-2024-03-24

RUN rustup install $RUST_TOOLCHAIN && rustup default $RUST_TOOLCHAIN && \
	rustup target add wasm32-unknown-unknown && \
	rustup component add rust-src rustfmt clippy && \
	# cargo install --git https://github.com/paritytech/try-runtime-cli --tag v0.3.3 --locked && \
	# cargo install subalfred --version 0.9.0-rc19 && \
	# cargo clean up
	# removes compilation artifacts cargo install creates (>250M)
	rm -rf "${CARGO_HOME}/registry" "${CARGO_HOME}/git"
	

# cache handler
ENV	RUSTC_WRAPPER=sccache \
# show backtraces
  	RUST_BACKTRACE=1