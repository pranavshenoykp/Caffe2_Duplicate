FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04
LABEL maintainer="aaronmarkham@fb.com"


########## NTT PROXIES ###################
ENV http_proxy http://proxy.rdh.ecl.ntt.co.jp:20066
ENV https_proxy http://proxy.rdh.ecl.ntt.co.jp:20066
ENV ftp_proxy http://proxy.rdh.ecl.ntt.co.jp:20066

# caffe2 install with gpu support

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    git \
    libgflags-dev \
    libgoogle-glog-dev \
    libgtest-dev \
    libiomp-dev \
    libleveldb-dev \
    liblmdb-dev \
    libopencv-dev \
    libopenmpi-dev \
    libprotobuf-dev \
    libsnappy-dev \
    openmpi-bin \
    openmpi-doc \
    protobuf-compiler \
    python-dev \
    python-numpy \
    python-pip \
    python-pydot \
    python-setuptools \
    python-scipy \
    wget \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir --upgrade pip==9.0.1 setuptools wheel && \
    pip install --no-cache-dir \
    flask \
    future \
    graphviz \
    hypothesis \
    jupyter \
    matplotlib \
    numpy \
    protobuf \
    pydot \
    python-nvd3 \
    pyyaml \
    requests \
    scikit-image \
    scipy \
    setuptools \
    six \
    tornado

########## INSTALLATION STEPS ###################
#RUN git clone --branch master --recursive https://github.com/caffe2/caffe2.git
COPY caffe2-master /tmp/caffe2-master
#RUN cd caffe2
#RUN some commnd to change .gitmodule
#RUN cd caffe2 && mkdir build && cd build \
RUN cd /tmp/caffe2-master/caffe2 && mkdir build && cd build \
    && cmake .. \
    -DCUDA_ARCH_NAME=Manual \
    -DCUDA_ARCH_BIN="35 52 60 61" \
    -DCUDA_ARCH_PTX="61" \
    -DUSE_NNPACK=OFF \
    -DUSE_ROCKSDB=OFF \
    && make -j"$(nproc)" install \
    && ldconfig \
    && make clean \
    && cd .. \
    && rm -rf build

ENV PYTHONPATH /usr/local
