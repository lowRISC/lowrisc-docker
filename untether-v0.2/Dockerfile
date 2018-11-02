# Version 0.2
# Untethered lowRISC SoC

# Pull base image
FROM lowrisc/lowrisc-docker:base-v16.04

# Make a working folder and set the necessary environment variables.
ENV DOCK /opt/docker
ENV RISCV /opt/riscv
RUN mkdir -p $RISCV

# VERSION
ENV TOOL_ENV untether-v0.2

# RISCV cross-compiler toolchain
WORKDIR $DOCK
RUN mkdir riscv-tools && \
    cd riscv-tools && \
    git init && \
    git remote add -t $TOOL_ENV origin https://github.com/lowrisc/riscv-tools.git && \
    git fetch && git checkout $TOOL_ENV && \
    git submodule update --init --recursive && \
    ./build.sh 2>&1 1>/dev/null && \
    cd $DOCK && rm -fr riscv-tools

# Set up env
ENV PATH $PATH:$RISCV/bin
