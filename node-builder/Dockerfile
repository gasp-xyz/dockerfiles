FROM rust:1.65-buster

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

RUN curl -L https://github.com/rui314/mold/releases/download/v1.7.0/mold-1.7.0-$(uname -m)-linux.tar.gz \
 | tar -C /usr/local --strip-components=1 -xzf - && ln -sf /usr/local/bin/mold "$(realpath /usr/bin/ld)"

WORKDIR /builds

ENV RUST_TOOLCHAIN=nightly-2022-10-10
RUN rustup install $RUST_TOOLCHAIN && rustup default $RUST_TOOLCHAIN

RUN cargo install sccache && rm -rf "${CARGO_HOME}/registry" "${CARGO_HOME}/git" /root/.cache/sccache

RUN rustup target add wasm32-unknown-unknown --toolchain $RUST_TOOLCHAIN

# cache handler
ENV	RUSTC_WRAPPER=sccache \
# show backtraces
  	RUST_BACKTRACE=1