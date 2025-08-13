FROM ubuntu:24.04 as build

ENV DEBIAN_FRONTEND=noninteractive

# Install basic dependencies
RUN apt-get update

RUN apt-get install -y wget curl unzip apt-utils && \
    apt-get install -y build-essential rustc cmake ninja-build ccache pkg-config && \
    apt-get install -y gcc g++ gfortran libblas-dev liblapack-dev && \
    apt-get install -y python3 python3-dev python3-pip python3-venv

### PAPI ###
RUN wget https://github.com/icl-utk-edu/papi/releases/download/papi-7-2-0-t/papi-7.2.0.tar.gz \
    && tar -xzf papi-7.2.0.tar.gz \
    && cd papi-7.2.0/src \
    && ./configure --with-tests=no \
    && make -j$(nproc) \
    && make install \
    && cd .. \
    && rm -rf papi-7.2.0 papi-7.2.0.tar.gz

### LibTorch ###
RUN wget https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.7.1%2Bcpu.zip \
    && unzip libtorch-cxx11-abi-shared-with-deps-2.7.1+cpu.zip \
    && rm libtorch-cxx11-abi-shared-with-deps-2.7.1+cpu.zip \
    && mv libtorch /usr/local/libtorch

### Final Image (Squash all layers) ###
FROM scratch

copy --from=build / /

ENV DEBIAN_FRONTEND=noninteractive

### PAPI ###
ENV __DAISY_PAPI_VERSION=0x07020000

### LibTorch ###
ENV LD_LIBRARY_PATH=/usr/local/libtorch/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
