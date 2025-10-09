FROM daisytuner/x64-base:latest AS build

ENV DEBIAN_FRONTEND=noninteractive

# Install TT_Metal
ARG TT_METAL_VERSION
RUN mkdir -p /tenstorrent && cd /tenstorrent && \
    git clone https://github.com/tenstorrent/tt-metal.git --depth 1 -b ${TT_METAL_VERSION} --recurse-submodules && \
    cd tt-metal && \
    ./install_dependencies.sh --docker && \
    ./build_metal.sh

ENV TT_METAL_HOME=/tenstorrent/tt-metal


