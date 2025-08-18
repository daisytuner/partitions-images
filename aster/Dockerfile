FROM ubuntu:24.04 AS build

ENV DEBIAN_FRONTEND=noninteractive

# Install basic dependencies
RUN apt-get update

RUN apt-get install -y wget curl unzip apt-utils software-properties-common && \
    apt-get install -y build-essential rustc cmake ninja-build ccache pkg-config && \
    apt-get install -y gcc g++ gfortran libblas-dev liblapack-dev && \
    apt-get install -y python3 python3-dev python3-pip python3-venv

### LLVM 19 ###
RUN apt-get update
RUN wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - \
    && add-apt-repository "deb http://apt.llvm.org/noble/ llvm-toolchain-noble-19 main" \
    && add-apt-repository "deb-src http://apt.llvm.org/noble/ llvm-toolchain-noble-19 main" \
    && apt-get update \
    && apt-get install -y llvm-19 clang-19 lld-19 libomp-19-dev clang-format-19

### CUDA ###
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/sbsa/cuda-keyring_1.1-1_all.deb \
    && dpkg -i cuda-keyring_1.1-1_all.deb \
    && rm cuda-keyring_1.1-1_all.deb \
    && apt-get update \
    && apt-get install -y cuda-toolkit-12-5
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

# Libtorch from pytorch install
RUN apt-get install -y python3-venv \
    && python3 -m venv venv \
    && venv/bin/pip install torch==2.7.1 --index-url https://download.pytorch.org/whl/cpu \
    && mkdir -p /usr/local/libtorch \
    && cp -r venv/lib/python3.12/site-packages/torch/lib venv/lib/python3.12/site-packages/torch/share venv/lib/python3.12/site-packages/torch/include /usr/local/libtorch \
    && cp -r venv/lib/python3.12/site-packages/torch.libs/* /usr/local/libtorch/lib \
    && rm -rf venv

### Final Image (Squash all layers) ###
FROM scratch

COPY --from=build / /

ENV DEBIAN_FRONTEND=noninteractive

### CUDA ###
ENV PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

### PAPI ###
ENV __DAISY_PAPI_VERSION=0x07020000

### LibTorch ###
ENV LD_LIBRARY_PATH=/usr/local/libtorch/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
