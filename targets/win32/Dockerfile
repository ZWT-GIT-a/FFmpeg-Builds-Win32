COPY --from=0 /usr/bin/* /usr/bin/

# 设置构建参数
ARG TARGET
ARG VARIANT

# 配置环境
ENV TARGET=$TARGET
ENV VARIANT=$VARIANT

# 安装依赖
RUN apt-get update && \
    apt-get install -y \
    gcc g++ make automake autoconf libtool \
    yasm nasm pkg-config libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# 复制构建脚本
COPY ./scripts.d /input/scripts.d

# 执行构建
RUN for s in /input/scripts.d/*.sh; do \
        chmod +x "$s"; \
        cp "$s" "/usr/bin/$(basename "$s" .sh)"; \
    done

# 启动构建流程
RUN run_stage.sh
