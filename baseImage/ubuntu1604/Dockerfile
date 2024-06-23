FROM ubuntu:16.04
LABEL Author="firmianay@gmail.com"
ARG MISE

# 设置 Tsinghua 镜像源（如果需要）
RUN if [ "$MISE" = "TSINGHUA" ]; then \
        echo "Using Tsinghua mirror" && \
        echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse" > /etc/apt/sources.list && \
        echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list && \
        echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list && \
        echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list; \
    else \
        echo "Using default mirror"; \
    fi

# 更新并安装基础包
RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends \
    python python-pip python-setuptools python-wheel \
    python3 python3-pip python3-setuptools python3-wheel

# 检查 pip 版本和更新 pip（如果需要）
RUN pip --version && python --version && python3 --version
RUN if [ "$MISE" = "TSINGHUA" ]; then \
        echo "Updating pip using Tsinghua mirror" && \
        pip install -i https://pypi.tuna.tsinghua.edu.cn/simple "pip < 21.0" -U && \
        pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple "pip < 21.0" -U && \
        pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple; \
    else \
        echo "Updating pip using default mirror" && \
        pip install "pip < 21.0" -U && \
        pip3 install "pip < 21.0" -U; \
    fi

# 安装其他工具
RUN apt-get install -y --no-install-recommends \
    iputils-ping vim-tiny net-tools nmap telnet && \
    rm -rf /var/lib/apt/lists/*

ENV PIP_NO_CACHE_DIR=1
