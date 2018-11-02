# Version 1.0
# lowRISC SoC with the upstream Rocket-chip

# Pull base image
FROM lowrisc/lowrisc-docker:base-v16.04

# Make a working folder and set the necessary environment variables.
ENV DOCK /opt/docker
ENV RISCV /opt/riscv
ENV VERI /opt/veri
RUN mkdir -p $RISCV

# VERSION
ENV TOOL_COMMIT 1272e77f207ed0c

# RISCV cross-compiler toolchain
WORKDIR $DOCK
RUN mkdir riscv-tools && \
    cd riscv-tools && \
    git init && \
    git remote add origin https://github.com/lowrisc/riscv-tools.git && \
    git fetch && git checkout $TOOL_COMMIT && \
    (git submodule update --init --recursive || \
    git submodule update --init --recursive || \
    git submodule update --init --recursive ) && \
    ./build.sh 2>&1 1>/dev/null && \
    cd $DOCK && rm -fr riscv-tools

# Set up env
ENV PATH $PATH:$RISCV/bin
