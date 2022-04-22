ARG IMAGE_NAME
FROM ${IMAGE_NAME}:dev AS builder

ENV SRC_DIR=/usr/src            \
    DIST_DIR=/dist              \
    LINUX_DIR=/usr/src/linux    \
    LINUX_REPO_URL=https://github.com/cloud-hypervisor/linux.git \
    CH_REPO=https://github.com/cloud-hypervisor/cloud-hypervisor.git \
    CH_DIR=/usr/src/ch

ARG KERNEL_VERSION
ARG CH_VERSION

RUN mkdir -p ${SRC_DIR} ${DIST_DIR} && \
    git clone --depth 1 --branch ch-${KERNEL_VERSION} ${LINUX_REPO_URL} ${LINUX_DIR} && \
    git clone --depth 1 --branch v${CH_VERSION} ${CH_REPO} ${CH_DIR} && \
    cd ${LINUX_DIR}

WORKDIR ${LINUX_DIR}

RUN cp ${CH_DIR}/resources/linux-config-x86_64 .config

RUN make bzImage -j `nproc`

RUN cp arch/x86/boot/compressed/vmlinux.bin /boot/vmlinux.bin && \
    cp .config /boot/config-${KERNEL_VERSION}

FROM scratch
COPY --from=builder /boot /