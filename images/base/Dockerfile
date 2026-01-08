FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# 使用官方源 + 超时重试机制
RUN echo 'APT::Acquire::Retries "3";' > /etc/apt/apt.conf.d/80-retries && \
    echo 'Acquire::http::Timeout "30";' >> /etc/apt/apt.conf.d/80-retries && \
    echo 'Acquire::https::Timeout "30";' >> /etc/apt/apt.conf.d/80-retries

RUN apt-get -y update --quiet && \
    apt-get -y dist-upgrade && \
    apt-get -y install build-essential yasm nasm \
    xxd pkgconf curl wget unzip git subversion mercurial \
    autoconf automake libtool libtool-bin autopoint gettext cmake clang meson ninja-build \
    texinfo texi2html help2man flex bison groff \
    gperf itstool ragel libc6-dev libssl-dev \
    gtk-doc-tools gobject-introspection gawk \
    ocaml ocamlbuild libnum-ocaml-dev indent p7zip-full \
    python3-distutils python3-jinja2 python3-jsonschema python3-apt python-is-python3 && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/* && \
    git config --global user.email "builder@localhost" && \
    git config --global user.name "Builder" && \
    git config --global advice.detachedHead false

ENV CARGO_HOME="/opt/cargo" RUSTUP_HOME="/opt/rustup" PATH="/opt/cargo/bin:${PATH}"
RUN curl https://sh.rustup.rs -sf | bash -s -- -y --no-modify-path && \
    cargo install cargo-c && rm -rf "${CARGO_HOME}"/registry "${CARGO_HOME}"/git

# 复制 scripts.d 到容器中
COPY scripts.d /input/scripts.d

# 挂载并复制脚本
RUN --mount=src=.,dst=/input \
    for s in /input/scripts.d/*.sh; do \
        cp "$s" "/usr/bin/$(basename "$s" .sh)"; \
    done
