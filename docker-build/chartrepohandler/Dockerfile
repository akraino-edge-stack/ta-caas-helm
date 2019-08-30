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

FROM alpine:3.9
MAINTAINER Krisztian Lengyel <krisztian.lengyel@nokia.com>

COPY mainstart.sh /usr/bin/mainstart.sh
COPY chart-repo-handler /chart-repo-handler
ENV GOPATH /build
ARG go_install_dir="/usr/local/go"
ARG go_version


# TODO remake build
RUN adduser -u 150 -D -H chartrepo \
&& apk add --no-cache --virtual .build-deps go gcc g++ git glide bash \
&& apk add --no-cache curl \
&& mkdir -p $go_install_dir \
&& curl -fsSL -k https://dl.google.com/go/go${go_version}.src.tar.gz | tar zx --strip-components=1 -C ${go_install_dir} \
&& cd ${go_install_dir}/src/ \
&& ./make.bash \
&& mkdir -p $GOPATH/src \
&& mv /chart-repo-handler $GOPATH/src/chart-repo-handler \
&& cd $GOPATH/src/chart-repo-handler \
&& chmod +x $GOPATH/src/chart-repo-handler/build.sh \
&& $GOPATH/src/chart-repo-handler/build.sh \
&& cp chart-repo-handler /usr/bin/chart-repo-handler \
&& rm -rf $GOPATH \
&& apk del .build-deps \
&& rm -rf /root/.glide \
&& chown -R chartrepo /usr/bin/mainstart.sh \
&& chmod +x /usr/bin/mainstart.sh

ENTRYPOINT /usr/bin/mainstart.sh
