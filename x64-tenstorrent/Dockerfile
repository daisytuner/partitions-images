FROM daisytuner/x64-base:latest AS build

ENV DEBIAN_FRONTEND=noninteractive

# Install TT_Metal
ARG TT_METAL_VERSION
RUN mkdir -p /tenstorrent && cd /tenstorrent && \
    wget https://github.com/tenstorrent/tt-metal/releases/download/v${TT_METAL_VERSION}/tt-metalium.tar.gz && \
    tar -xzf tt-metalium.tar.gz && \
    rm tt-metalium.tar.gz && \
    cd tt-metalium && \
    ./install_dependencies.sh --docker && \
    cmake -B build_Release -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=build_Release -DCMAKE_EXPORT_COMPILE_COMMANDS=OFF -DTT_UNITY_BUILDS=ON -DTT_ENABLE_LIGHT_METAL_TRACE=ON -DTT_ENABLE_LIGHT_METAL_TRACE=ON -DWITH_PYTHON_BINDINGS=ON -DPython3_EXECUTABLE=/usr/bin/python3 -DPython3_INCLUDE_DIR=/usr/include/python3.12 -DPython3_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.12.so -DENABLE_DISTRIBUTED=ON -DENABLE_FAKE_KERNELS_TARGET=OFF -DCMAKE_TOOLCHAIN_FILE=cmake/x86_64-linux-clang-17-libstdcpp-toolchain.cmake && \
    cmake --build build_Release --target install && \
    ln -nsf build_Release build

ENV TT_METAL_HOME=/tenstorrent/tt-metalium


