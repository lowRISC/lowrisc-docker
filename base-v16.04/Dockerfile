# Version 1.0
# lowRISC SoC with the upstream Rocket-chip

# Pull base image
FROM ubuntu:16.04

# Install some base tools
RUN apt-get update 2>&1 1>/dev/null && \
    apt-get install -y \
    automake \
    autotools-dev \
    curl \
    libmpc-dev \
    gawk \
    build-essential \
    bison \
    flex \
    texinfo \
    gperf \
    git \
    openjdk-8-jre \
    openjdk-8-jdk \
    python \
    libboost-dev \
    device-tree-compiler \
    zlib1g-dev \
    2>&1 1>/dev/null && \
    apt-get clean

# Make a working folder and set the necessary environment variables.
ENV DOCK /opt/docker
ENV VERI /opt/veri
RUN mkdir -p $DOCK

# VERSION
ENV VERI_COMMIT a69936de744dfa9

# Verilator
WORKDIR $DOCK
RUN rm -fr $VERI && \
    git clone http://git.veripool.org/git/verilator && \
    cd verilator && git checkout $VERI_COMMIT && \
    autoconf && ./configure --prefix=$VERI && \
    make 2>&1 1>/dev/null && \
    make install && \
    ln -s share/verilator/include $VERI/include && \
    ln -s ../share/verilator/bin/verilator_includer $VERI/bin/verilator_includer && \
    cd $DOCK && rm -fr verilator

# Set up env
ENV PATH $PATH:$VERI/bin
ENV VERILATOR_ROOT $VERI
