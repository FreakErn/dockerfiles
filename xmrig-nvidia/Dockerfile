ARG CUDA_VERSION=10.2
ARG BUILD_FLAVOR=devel
ARG RUN_FLAVOR=base
ARG DISTRO=ubuntu18.04

FROM nvidia/cuda:${CUDA_VERSION}-${BUILD_FLAVOR}-${DISTRO} AS xmrig

ENV XMRIG_CUDA_VERSION 'master'

RUN apt-get update \
    && apt-get -y install --no-install-recommends cmake g++ gcc git libc++-dev libssl-dev make libuv1-dev \
    && cd / \
    && git clone https://github.com/xmrig/xmrig-cuda.git \
    && cd xmrig-cuda \
    && git checkout ${XMRIG_CUDA_VERSION} \
    && mkdir build \
    && cd build 

ENV CMAKE_OPTS '-DCUDA_LIB=/usr/local/cuda/lib64/stubs/libcuda.so'

RUN cd /xmrig-cuda/build \
    && cmake .. ${CMAKE_OPTS} 
RUN cd /xmrig-cuda/build \
    && make -j$(nproc)


ENV XMRIG_VERSION 'master'

RUN git clone https://github.com/xmrig/xmrig \
    && cd /xmrig \
    && git checkout ${XMRIG_VERSION} \
    && mkdir build 

ENV CMAKE_OPTS '-DWITH_HWLOC=OFF'

RUN cd /xmrig/build \
    && cmake .. ${CMAKE_OPTS} 
RUN cd /xmrig/build \
    && make -j$(nproc)

#FROM nvidia/cuda:${CUDA_VERSION}-${RUN_FLAVOR}-${DISTRO}
FROM nvidia/cuda:${CUDA_VERSION}-${BUILD_FLAVOR}-${DISTRO}

LABEL maintainer='docker@freakern.de'

COPY --from=xmrig /xmrig-cuda/build/* /usr/local/bin/xmrig-cuda/
COPY --from=xmrig /xmrig/build/* /usr/local/bin/xmrig/

RUN apt-get update \
    && apt-get -y install --no-install-recommends gcc libc++1 libnvidia-compute-440

WORKDIR /usr/local/bin
RUN echo '#!/bin/bash\n\n/usr/local/bin/xmrig/xmrig --cuda --cuda-loader="/usr/local/bin/xmrig-cuda/libxmrig-cuda.so" $@' > xmrig.sh  \
    && chmod +x xmrig.sh
ENTRYPOINT ['/usr/local/bin/xmrig/xmrig --cuda-loader="/usr/local/bin/xmrig-cuda/libxmrig-cuda.so"']
#ENTRYPOINT ["/usr/local/bin/xmrig/xmrig"]
ENTRYPOINT ["/usr/local/bin/xmrig.sh"]
