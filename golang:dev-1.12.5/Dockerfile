FROM golang:1.12.5

RUN apt-get -y update && apt-get -y install --no-install-recommends \
wget \
g++ \
openssl \
git \
vim \
pkg-config

COPY package/node-v10.15.3.tar.gz /tmp/node-v10.15.3.tar.gz

RUN cd /tmp \
&& tar zxvf node-v10.15.3.tar.gz \
&& cd node-v10.15.3 \
&& ./configure \
&& make \
&& make install

RUN go get github.com/tools/godep \
&& npm config set registry https://registry.npm.taobao.org \
&& apt-get autoremove -y \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
&& apt-get clean

EXPOSE 80