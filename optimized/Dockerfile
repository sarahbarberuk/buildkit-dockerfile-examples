FROM ubuntu:latest as base
 
ENV CGO_ENABLED=1
ENV GOPATH=$HOME/go
ENV PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
ENV YARN_IGNORE_NODE=1
ENV NODE_OPTIONS=--max_old_space_size=10000

RUN apt-get update

FROM base as curl
RUN apt-get install -y curl

FROM curl as golang
RUN curl -O \
    https://storage.googleapis.com/golang/go1.21.1.linux-amd64.tar.gz \
    && tar -xvf go1.21.1.linux-amd64.tar.gz  

FROM base as grafana   
RUN apt-get install -y git \
    && git clone https://github.com/grafana/grafana.git grafana/ 


FROM curl
COPY --from=golang go /usr/local
COPY --from=grafana grafana/ grafana/
RUN apt-get install -y make build-essential \
    && curl \
    https://raw.githubusercontent.com/creationix/nvm/master/install.sh \
    | bash \
    && . ~/.nvm/nvm.sh \
    && nvm install 16.18.0 \
    && npm install --global yarn \
    && cd grafana/ \
    && yarn install \
    && make build
