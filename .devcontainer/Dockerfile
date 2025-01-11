FROM mcr.microsoft.com/devcontainers/base:debian-12

ENV DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-c"]

RUN apt update && \
    apt install -y curl && \
    echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/3/Debian_12/ /' | tee /etc/apt/sources.list.d/shells:fish:release:3.list && \
    curl -fsSL https://download.opensuse.org/repositories/shells:fish:release:3/Debian_12/Release.key | gpg --dearmor | tee /etc/apt/trusted.gpg.d/shells_fish_release_3.gpg > /dev/null && \
    apt update && \
    apt install -y \
        git \
        xdg-utils \
        fish && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

USER vscode
WORKDIR "/work"
ENV HOME="/home/vscode"

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="${HOME}/.cargo/bin:${PATH}"
RUN rustup install 1.83.0 && \
    rustup default 1.83.0 && \
    rustup target add wasm32-unknown-unknown && \
    rustup target add wasm32-unknown-emscripten && \
    rustup target add wasm32-wasip1 && \
    rustup target add wasm32-wasip1-threads && \
    rustup target add wasm32-wasip2 && \
    cargo install wasm-tools@1.222.0 && \
    cargo install pest-language-server@0.3.11

RUN curl https://get.volta.sh | bash
ENV PATH="${HOME}/.volta/bin:${PATH}"
RUN volta install \
    node@22.12.0 \
    npm@11.0.0 \
    sort-package-json@2.12.0 \
    tsx@4.19.2 \
    typescript@5.6.2

ENV FISH_CONFIG_DIR="${HOME}/.config/fish"
ENV FISH_CONFIG_FILE="${FISH_CONFIG_DIR}/config.fish"
RUN mkdir -p "${FISH_CONFIG_DIR}" && \
    echo "functions --erase fish_greeting" >> "${FISH_CONFIG_FILE}" && \
    echo "alias ll='ls -alF'" >> "${FISH_CONFIG_FILE}"
