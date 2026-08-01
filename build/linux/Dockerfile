# VSCodium Build Docker Container
FROM ubuntu:22.04

LABEL maintainer="VSCodium Team"
LABEL description="Docker container for building VSCodium with Llama.cpp support"

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC

# Install build dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    gnupg \
    ca-certificates \
    build-essential \
    g++ \
    gcc \
    make \
    python3 \
    python3-pip \
    nodejs \
    npm \
    libx11-dev \
    libxkbfile-dev \
    libsecret-1-dev \
    libkrb5-dev \
    rpm \
    fakeroot \
    xz-utils \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /workspace

# Copy build scripts
COPY . /workspace/

# Install Node.js dependencies
RUN npm config set registry https://registry.npmjs.org/ && \
    npm install -g yarn gulp-cli && \
    yarn install --frozen-lockfile || true

# Build arguments for customization
ARG BUILD_TYPE=stable
ARG ENABLE_LLAMA_CPP=true

# Set environment variables
ENV VSCODIUM_BUILD_TYPE=${BUILD_TYPE}
ENV LLAMA_CPP_ENABLED=${ENABLE_LLAMA_CPP}

# Default build command
CMD ["./build.sh"]
