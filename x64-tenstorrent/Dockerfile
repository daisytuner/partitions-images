FROM daisytuner/x64-base:latest AS build

ENV DEBIAN_FRONTEND=noninteractive

# Install TT_Metal
ARG TT_METAL_VERSION
RUN mkdir -p /tenstorrent && cd /tenstorrent && \
    wget https://github.com/tenstorrent/tt-metal/releases/download/${TT_METAL_VERSION}/tt-metalium.tar.gz && \
    tar -xzf tt-metalium.tar.gz && \
    rm tt-metalium.tar.gz && \
    cd tt-metalium && \
    ./install_dependencies.sh --docker && \
    ./build_metal.sh

ENV TT_METAL_HOME=/tenstorrent/tt-metalium


