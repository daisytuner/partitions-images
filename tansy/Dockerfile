FROM nvcr.io/nvidia/l4t-jetpack:r36.4.0

ENV DEBIAN_FRONTEND=noninteractive

# Install basic dependencies
RUN apt-get update

RUN apt-get install -y wget curl unzip apt-utils && \
    apt-get install -y build-essential rustc cmake ninja-build ccache pkg-config && \
    apt-get install -y gcc g++ gfortran libblas-dev liblapack-dev && \
    apt-get install -y python3 python3-dev python3-pip python3-venv

### CUDA ###
ENV PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

### PAPI ###
RUN wget https://github.com/icl-utk-edu/papi/releases/download/papi-7-2-0-t/papi-7.2.0.tar.gz \
    && tar -xzf papi-7.2.0.tar.gz \
    && cd papi-7.2.0/src \
    && ./configure --with-components="cuda nvml" --with-tests=no \
    && make -j$(nproc) \
    && make install \
    && cd .. \
    && rm -rf papi-7.2.0 papi-7.2.0.tar.gz
ENV __DAISY_PAPI_VERSION=0x07020000

### LibTorch ###
RUN wget https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.7.1%2Bcpu.zip \
    && unzip libtorch-cxx11-abi-shared-with-deps-2.7.1+cpu.zip \
    && rm libtorch-cxx11-abi-shared-with-deps-2.7.1+cpu.zip \
    && mv libtorch /usr/local/libtorch
ENV LD_LIBRARY_PATH=/usr/local/libtorch/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
