FROM ubuntu:24.04 as build

ENV DEBIAN_FRONTEND=noninteractive

# Install basic dependencies
RUN apt-get update

RUN apt-get install -y wget curl apt-utils && \
    apt-get install -y build-essential rustc cmake ninja-build ccache pkg-config && \
    apt-get install -y gcc g++ gfortran libblas-dev liblapack-dev && \
    apt-get install -y python3 python3-dev python3-pip python3-venv

### Profiling Tools ###

# PAPI
RUN wget https://github.com/icl-utk-edu/papi/releases/download/papi-7-2-0-t/papi-7.2.0.tar.gz \
    && tar -xzf papi-7.2.0.tar.gz \
    && cd papi-7.2.0/src \
    && ./configure --with-tests=no \
    && make -j$(nproc) \
    && make install \
    && cd .. \
    && rm -rf papi-7.2.0 papi-7.2.0.tar.gz

### PySpy ###
RUN python3 -m pip install py-spy --break-system-packages

### Final Image (Squash all layers) ###
FROM scratch

copy --from=build / /

ENV DEBIAN_FRONTEND=noninteractive

### PAPI ###
ENV __DAISY_PAPI_VERSION=0x07020000
