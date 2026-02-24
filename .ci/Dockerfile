FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive \
    LC_ALL=C.UTF-8 \
    LANG=C.UTF-8


RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates curl git unzip xz-utils zip \
      bash sudo \
      build-essential clang cmake ninja-build pkg-config libgtk-3-dev \
      libglu1-mesa \
    && rm -rf /var/lib/apt/lists/*


ARG FLUTTER_VERSION=3.38.9
RUN mkdir -p /opt/flutter/versions /opt/flutter/bin
# pobranie stabilnego archiwum (bez całego gita)
RUN curl -fsSL "https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_${FLUTTER_VERSION}-stable.tar.xz" \
    -o /tmp/flutter.tar.xz \
 && tar -C /opt/flutter/versions -xJf /tmp/flutter.tar.xz \
 && rm /tmp/flutter.tar.xz \
 # archiwum rozpakowuje do folderu "flutter"
 && mv /opt/flutter/versions/flutter "/opt/flutter/versions/flutter-${FLUTTER_VERSION}" \
 && ln -sfn "/opt/flutter/versions/flutter-${FLUTTER_VERSION}" /opt/flutter/current


ENV PATH="/opt/flutter/current/bin:${PATH}"
RUN flutter config --no-analytics \
 && flutter precache --linux \
 && flutter doctor -v || true


RUN useradd -m -u 1000 -s /bin/bash build \
 && echo "build ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/build && chmod 0440 /etc/sudoers.d/build \
 && mkdir -p /home/build/.pub-cache && chown -R build:build /home/build /opt/flutter
ENV PUB_CACHE=/home/build/.pub-cache
USER build
WORKDIR /workspace

