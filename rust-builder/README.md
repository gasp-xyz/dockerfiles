# Node Builder image to be used in CI workflows of `gasp-monorepo` repository

This image is being built for multiple platform so that it can be used on ARM-based local development machines.

## Pre-requisites

On the host where you build you'll need to run next commands before running build:

```bash
docker run --privileged --rm tonistiigi/binfmt --install all
docker buildx create --use --name custom-builder
```

## Build command

```bash
# Latest:
docker buildx build --platform linux/arm64,linux/amd64 -t gaspxyz/rust-builder:multi --push .
# Specific toolchain:
export RUST_VERSION=1.77
export RUST_TOOLCHAIN=nightly-2024-01-20
docker buildx build --platform linux/arm64,linux/amd64 --build-arg RUST_TOOLCHAIN=${RUST_TOOLCHAIN} --build-arg RUST_VERSION=${RUST_VERSION} -t gaspxyz/rust-builder:multi-${RUST_VERSION}-${RUST_TOOLCHAIN} --push .
```

## Some caveats

This image requires ~40Gb RAM to be available to build because of the cargo compilations for the different platforms for SubAlfred.
If you'll experience issues with building it, getting errors during compilation next links may be helpful:

- <https://github.com/docker/buildx/issues/495#issuecomment-761562905>
