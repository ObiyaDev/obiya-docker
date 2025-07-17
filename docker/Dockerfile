FROM debian:bullseye-slim

# Environment variables
ENV NODE_VERSION=22.17.1\
    NPM_VERSION=10.9.2\
    NODE_HOME=/usr/local/node\
    MOTIA_DOCKER=true

# System packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    zlib1g-dev \
    libncurses5-dev \
    libgdbm-dev \
    libnss3-dev \
    libssl-dev \
    libreadline-dev \
    libffi-dev \
    libsqlite3-dev \
    libbz2-dev \
    git \
    wget \
    curl \
    ca-certificates \
    xz-utils \
    ruby-full \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Setup Ruby bundler
RUN gem install bundler

# Setup Python

RUN wget https://www.python.org/ftp/python/3.13.4/Python-3.13.4.tar.xz \
  && tar -xf Python-3.13.4.tar.xz \
  && cd Python-3.13.4 \
  && ./configure --enable-optimizations \
  && make -j$(nproc) \
  && make altinstall \
  && cd .. \
  && rm -rf Python-3.13.4 Python-3.13.4.tar.xz \
  && ln -s /usr/local/bin/python3.13 /usr/bin/python3.13 \
  && ln -s /usr/local/bin/python3.13 /usr/bin/python \
  && ln -s /usr/local/bin/pip3.13 /usr/bin/pip \
  && ln -s /usr/local/bin/pip3.13 /usr/bin/pip3

# Create directory for Node
RUN mkdir -p ${NODE_HOME}

# Setup Node
RUN set -ex \
    && ARCH=$(uname -m) \
    && if [ "$ARCH" = "x86_64" ]; then \
        NODE_ARCH="x64"; \
       elif [ "$ARCH" = "aarch64" ] || [ "$ARCH" = "arm64" ]; then \
        NODE_ARCH="arm64"; \
       else \
        echo "Unsupported architecture: $ARCH"; exit 1; \
       fi \
    && echo "Installing Node.js for ${NODE_ARCH} architecture" \
    && cd /tmp \
    && NODE_TAR="node-v${NODE_VERSION}-linux-${NODE_ARCH}.tar.xz" \
    && curl -fsSLO --compressed "https://nodejs.org/dist/v${NODE_VERSION}/${NODE_TAR}" \
    && tar -xJf "${NODE_TAR}" -C "${NODE_HOME}" --strip-components=1 --no-same-owner \
    && rm "${NODE_TAR}" \
    && ln -s "${NODE_HOME}/bin/node" /usr/local/bin/node \
    && ln -s "${NODE_HOME}/bin/npm" /usr/local/bin/npm \
    && ln -s "${NODE_HOME}/bin/npx" /usr/local/bin/npx \
    && npm install -g "npm@${NPM_VERSION}" \
    && npm cache clean --force

# Working directory
WORKDIR /app

## Default command which will be overwritten in the Dockerfile implementing this image
CMD ["echo", "Please implement a Dockerfile using this image as a base"]