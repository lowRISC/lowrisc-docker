# Version 0.4
# lowRISC SoC with a minion and tagged pipeline

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
    python \
    libboost-dev \
    2>&1 1>/dev/null && \
    apt-get clean

# Make a working folder and set the necessary environment variables.
ENV DOCK /opt/docker
ENV RISCV /opt/riscv
ENV VERI /opt/veri
RUN mkdir -p $DOCK $RISCV

# VERSION
ENV TOOL_COMMIT d86cf0e
ENV VERI_COMMIT verilator_3_902

# Add the GNU utils bin folder to the path.
ENV PATH $RISCV/bin:$PATH

# RISCV cross-compiler toolchain
WORKDIR $DOCK
RUN mkdir riscv-tools && \
    cd riscv-tools && \
    git init && \
    git remote add origin https://github.com/lowrisc/riscv-tools.git && \
    git fetch && git checkout $TOOL_COMMIT && \
    git submodule update --init --recursive && \
    ./build.sh 2>&1 1>/dev/null && \
    cd riscv-gnu-toolchain/build && \

# Verilator
WORKDIR $DOCK
RUN git clone http://git.veripool.org/git/verilator && \
    cd verilator && git checkout $VERI_COMMIT && \
    autoconf && ./configure --prefix=$VERI && \
    make 2>&1 1>/dev/null && \
    make install && \
    ln -s share/verilator/include $VERI/include && \
    ln -s ../share/verilator/bin/verilator_includer $VERI/bin/verilator_includer && \
    cd $DOCK && rm -fr verilator

# Set up env
ENV PATH $PATH:$RISCV/bin:$VERI/bin
ENV VERILATOR_ROOT $VERI
