ARG DRIVER_VERSION="85c88952b018fdbce2464222c3303229f5bfcfad"
ARG DRIVER_NAME="falco"
ARG KERNEL_VERSION="4.14.186-146.268.amzn2.x86_64"

FROM amazonlinux:2 AS builder
ARG DRIVER_VERSION
ARG DRIVER_NAME
ARG KERNEL_VERSION

ENV DRIVER_VERSION ${DRIVER_VERSION}
ENV DRIVER_NAME ${DRIVER_NAME}
ENV DRIVER_DEVICE_NAME ${DRIVER_NAME}
ENV KERNEL_VERSION ${KERNEL_VERSION}

ENV KERNELDIR "/usr/src/kernels/${KERNEL_VERSION}/"
WORKDIR /root
ADD templates/ templates/
ADD https://github.com/draios/sysdig/archive/${DRIVER_VERSION}.tar.gz ./
RUN yum install -y tar gzip gettext make kernel-${KERNEL_VERSION} kernel-devel-${KERNEL_VERSION}
RUN tar -xzf ${DRIVER_VERSION}.tar.gz
RUN mkdir driver
RUN mv sysdig-${DRIVER_VERSION}/driver/* driver/
RUN rm -rf sysdig-${DRIVER_VERSION} ${DRIVER_VERSION}.tar.gz
RUN envsubst < "templates/driver_config.h.template" > "driver/driver_config.h"
RUN envsubst < "templates/Makefile.template" > "driver/Makefile"
WORKDIR /root/driver
RUN make

FROM falcosecurity/falco:latest
ARG DRIVER_VERSION
ARG DRIVER_NAME
ARG DRIVER_DEVICE_NAME
ARG KERNEL_VERSION

COPY --from=builder /root/driver/falco.ko /root/.falco/${DRIVER_NAME}_amazonlinux2_${KERNEL_VERSION}_1.ko
