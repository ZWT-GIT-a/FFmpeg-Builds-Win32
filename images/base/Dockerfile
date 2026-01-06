FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive

RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse" > /etc/apt/sources.list && \
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse" >> /etc/apt/sources.list && \
    apt-get -y update --quiet && \
    apt-get -y dist-upgrade && \
    apt-get -y install build-essential yasm nasm \
    xxd pkgconf curl wget unzip git subversion mercurial \
    autoconf automake libtool libtool-bin autopoint gettext cmake clang meson ninja-build \
    texinfo texi2html help2man flex bison groff \
    gperf itstool ragel libc6-dev libssl-dev \
    gtk-doc-tools goobject-introspection gawk \
    ocaml ocamlbuild libnum-ocaml-dev indent p7zip-full \
    python3-distutils python3-jinja2 python3-jsonschema python3-apt python-is-python3 && \
    apt-get -y clean && \
    git config --global user.email "builder@localhost" && \
    git config --global user.name "Builder" && \
    git config --global advice.detachedHead false

ENV CARGO_HOME="/opt/cargo" RUSTUP_HOME="/opt/rustup" PATH="/opt/cargo/bin:${PATH}"
RUN curl https://sh.rustup.rs -sf | bash -s -- -y --no-modify-path && \
    cargo install cargo-c && rm -rf "${CARGO_HOME}"/registry "${CARGO_HOME}"/git

RUN --mount=src=.,dst=/input \
    for s in /input/*.sh; do cp $s /usr/bin/$(echo $s | sed -e 's|\.sh$||' -e 's|\/|_|'); done
