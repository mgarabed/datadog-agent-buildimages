FROM ubuntu:16.04

ARG CMAKE_VERSION=3.14.4
ARG CLANG_VERSION=8.0.0

# Environment
ENV CMAKE_VERSION $CMAKE_VERSION
ENV CLANG_VERSION $CLANG_VERSION

RUN apt-get update && apt-get install -y fakeroot curl git procps bzip2 \
        python2.7-dev build-essential pkg-config tar libsystemd-dev libkrb5-dev \
        python3-dev gettext libtool autopoint autoconf libtool-bin \
        selinux-basics

## CMAKE
RUN mkdir /src
RUN cd /src && \
        curl -sL -O https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}.tar.gz && \
        tar xf cmake-${CMAKE_VERSION}.tar.gz && \
        cd cmake-${CMAKE_VERSION} && \
        ./bootstrap --prefix=/opt/cmake && \
        make -j4 && \
        make install && \
        rm -rf /src

## CLANG - WARNING this is ~4 hours!
RUN mkdir /src
RUN cd /src &&  curl -LO http://releases.llvm.org/${CLANG_VERSION}/llvm-${CLANG_VERSION}.src.tar.xz && \
        tar -xf llvm-${CLANG_VERSION}.src.tar.xz && \
        curl -LO http://releases.llvm.org/${CLANG_VERSION}/cfe-${CLANG_VERSION}.src.tar.xz && \
        tar -xf cfe-${CLANG_VERSION}.src.tar.xz && \
        mv cfe-${CLANG_VERSION}.src/ clang && \
        cd llvm-${CLANG_VERSION}.src && \
        export PATH=/opt/cmake/bin:$PATH && \
        mkdir build && \
        cd build && \
        cmake -DCMAKE_BUILD_TYPE=release -DLLVM_ENABLE_PROJECTS=clang -DLLVM_TEMPORARILY_ALLOW_OLD_TOOLCHAIN=ON -DCMAKE_INSTALL_PREFIX=/opt/clang .. && \
        make -j4 && \
        make install && \
        rm -rf /src
