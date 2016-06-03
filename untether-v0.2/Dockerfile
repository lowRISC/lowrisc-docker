# Version 0.2
# Untethered lowRISC SoC

# Pull base image
FROM ubuntu:14.04

# Install some base tools
RUN apt-get update 2>&1 1>/dev/null

RUN apt-get install -y \
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
    openjdk-7-jre \
    libboost-dev \
    2>&1 1>/dev/null

RUN apt-get clean

# Make a working folder and set the necessary environment variables.
ENV DOCK /opt/docker
ENV RISCV /opt/riscv
ENV VERI /opt/veri
RUN mkdir -p $DOCK
RUN mkdir -p $RISCV

# VERSION
ENV TOOL_ENV untether-v0.2
ENV VERI_ENV 80678ae7bc4d7111e

# Add the GNU utils bin folder to the path.
ENV PATH $RISCV/bin:$PATH

# RISCV cross-compiler toolchain
WORKDIR $DOCK
RUN mkdir riscv-tools && \
    cd riscv-tools && \
    git init && \
    git remote add -t $TOOL_ENV origin https://github.com/lowrisc/riscv-tools.git && \
    git fetch && git checkout $TOOL_ENV && \
    git submodule update --init --recursive && \
    ./build.sh 2>&1 1>build.log && \
    tail build.log && \
    cd riscv-gnu-toolchain/build && \
    make -j16 linux 2>&1 1>build.log && \
    tail build.log && \
    cd - && \
    rm -fr riscv-gnu-toolchain/build riscv-fesvr/build riscv-isa-sim/build riscv-pk/build

# Verilator
WORKDIR $DOCK
RUN git clone http://git.veripool.org/git/verilator && \
    cd verilator && git checkout $VERI_ENV && \
    autoconf && ./configure --prefix=$VERI && \
    make -j16 2>&1 1>build.log && \
    make install && \
    ln -s share/verilator/include $VERI/include && \
    ln -s ../share/verilator/bin/verilator_includer $VERI/bin/verilator_includer && \
    cd ..; rm -fr verilator

# Set up env
ENV PATH $PATH:$RISCV/bin:$VERI/bin
ENV VERILATOR_ROOT $VERI
