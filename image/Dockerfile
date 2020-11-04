FROM nginx:mainline

ENV NPM_CONFIG_LOGLEVEL='info' \
    NODE_VERSION='14.15.0' \
    YARN_VERSION='1.22.5'

RUN apt-get update && \
    apt-get install --no-install-recommends --no-install-suggests -y \
    openssh-client \
    git \
    curl \
    bzip2 \
    xz-utils \
    ca-certificates \
    g++ \
    build-essential \
    python3 \
	&& apt-get -y --purge autoremove \
  && rm -rf /var/lib/apt/lists/*

RUN groupadd --gid 1000 node \
    && useradd --uid 1000 --gid node --shell /bin/bash --create-home node

RUN curl -SLO "http://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
	&& tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
	&& rm "node-v$NODE_VERSION-linux-x64.tar.xz" \
	&& ln -s /usr/local/bin/node /usr/local/bin/nodejs

RUN echo $YARN_VERSION
RUN curl -fSLO --compressed "http://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz" \
	&& mkdir -p /opt/yarn \
	&& tar -xzf yarn-v$YARN_VERSION.tar.gz -C /opt/yarn --strip-components=1 \
	&& ln -s /opt/yarn/bin/yarn /usr/local/bin/yarn \
	&& ln -s /opt/yarn/bin/yarn /usr/local/bin/yarnpkg \
	&& rm yarn-v$YARN_VERSION.tar.gz


RUN yarn global add @vue/cli @vue/cli-service-global typescript

COPY package.json /code/
COPY image/nginx.conf /etc/nginx/

WORKDIR /code

RUN yarn

COPY . /code

CMD yarn serve

EXPOSE 8080 3000 80 443
