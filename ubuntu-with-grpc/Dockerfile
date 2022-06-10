FROM ubuntu:latest AS base

RUN apt update && apt upgrade -y && apt install -y cmake git build-essential autoconf libtool pkg-config libboost-dev

FROM base AS builder

RUN mkdir /src

RUN cd /src && \
    git clone https://github.com/grpc/grpc.git && \
    cd /src/grpc && \
    git submodule update --init --recursive

RUN mkdir -p /src/grpc/cmake/build && cd /src/grpc/cmake/build && cmake -DgRPC_INSTALL=ON /src/grpc && make

FROM base AS image

COPY --from=builder /src/grpc /src/grpc

RUN cd /src/grpc/cmake/build && make install

RUN rm -rf /src/grpc