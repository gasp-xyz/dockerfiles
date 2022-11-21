# Node Builder image to be used in CI workflows of `mangata-node` repository
This image is being built for multiple platform so that it can be used on ARM-based local development machines.

## Build command
```
# Latest:
docker buildx build --platform linux/arm64,linux/amd64 -t mangatasolutions/node-builder:multi --push .
# Specific toolchain:
export RUST_TOOLCHAIN=nightly-2022-11-21
docker buildx build --platform linux/arm64,linux/amd64 --build-arg RUST_TOOLCHAIN=${RUST_TOOLCHAIN} -t mangatasolutions/node-builder:multi-${RUST_TOOLCHAIN} --push .
```