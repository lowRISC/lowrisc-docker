# Version 0.4
# lowRISC SoC with a minion and tagged pipeline

# Pull base image
FROM lowrisc/lowrisc-docker:base-v16.04

# Make a working folder and set the necessary environment variables.
ENV DOCK /opt/docker
ENV RISCV /opt/riscv
RUN mkdir -p $RISCV

# VERSION
ENV TOOL_COMMIT d86cf0e

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

# Set up env
ENV PATH $PATH:$RISCV/bin
ENV VERILATOR_ROOT $VERI
