# 多阶段构建：从 base-image 获取工具
COPY --from=0 /usr/bin/* /usr/bin/

# 设置构建参数
ARG TARGET
ARG VARIANT

# 安装依赖
RUN apt-get update && \
    apt-get install -y \
    gcc g++ make automake autoconf libtool \
    yasm nasm pkg-config libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# 执行构建流程
RUN run_stage.sh
