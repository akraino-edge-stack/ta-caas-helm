# Copyright 2019 Nokia
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

ARG CENTOS_BASE
FROM $CENTOS_BASE
MAINTAINER Krisztian Lengyel <krisztian.lengyel@nokia.com>

ARG HELM_VERSION
ARG go_version
ARG binaries
ARG go_install_dir="/usr/local/go"

ENV GOBIN=/usr/local/bin
ENV GOPATH /build
ARG build_packages="gcc gcc-c++ glibc-devel make autoconf git which libtool"
ENV PATH="$GOPATH/bin:$go_install_dir/bin:$PATH"

RUN yum install -y --setopt=skip_missing_names_on_install=False \
      ${build_packages} \
&&  mkdir -p $go_install_dir \
&&  if [ $(uname -m) == 'aarch64' ]; then HOST_ARCH=arm64; else HOST_ARCH=amd64; fi \
&&  curl -fsSL -k https://dl.google.com/go/go${go_version}.linux-${HOST_ARCH}.tar.gz | tar zx --strip-components=1 -C ${go_install_dir} \
\
# Build Helm
&&  mkdir -p $GOPATH/src/k8s.io \
&&  cd $GOPATH/src/k8s.io \
&&  git clone https://github.com/kubernetes/helm.git \
&&  cd helm \
&&  make GIT_TAG=v${HELM_VERSION} bootstrap build-cross \
&&  mkdir -p ${binaries} \
&&  mv ./_dist/linux-${HOST_ARCH}/tiller ${binaries}/ \
&&  mv ./_dist/linux-${HOST_ARCH}/helm ${binaries}/ \
&&  rm -rf $GOPATH \
&&  rm -rf $go_install_dir \
&&  rm -rf /build \
&&  rm -rf /root/.glide
