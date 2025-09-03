FROM node:22-trixie-slim

ENV DEBIAN_FRONTEND=noninteractive

# 添加旧版 Python2 的兼容支持（来自 snapshot.debian.org）
RUN echo "deb http://archive.debian.org/debian buster main" >> /etc/apt/sources.list && \
    apt-get -qq update || true && \
    apt-get -y -o Acquire::Check-Valid-Until=false install \
    python2-minimal \
    python-is-python2 || true

RUN apt-get -qq update && \
    apt-get install -y \
    gcc \
    g++ \
    fpc \
    fp-compiler \
    default-jdk-headless \
    rustc \
    ghc \
    cabal-install \
    libjavascriptcoregtk-4.0-bin \
    golang \
    ruby \
    mono-runtime \
    mono-mcs \
    kotlin \
    php \
    php-cli \
    nodejs \
    bash \
    python3 \
    pypy3 \
    wget \
    curl \
    ca-certificates && \
    # 建立 python 命令的兼容软链接
    update-alternatives --install /usr/bin/python python /usr/bin/python2 1 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3 2 && \
    # 验证各语言工具链是否正确安装
    gcc --version && \
    g++ --version && \
    fpc -iV && \
    javac -version && \
    rustc --version && \
    ghc --version && \
    cabal --version && \
    go version && \
    ruby --version && \
    mono --version && \
    kotlinc -version && \
    php --version && \
    node --version && \
    python2 --version && \
    python3 --version && \
    pypy3 --version

ADD ./entrypoint.sh /root/entrypoint.sh
ADD ./judge.yaml /root/judge.yaml
ADD ./judge.yaml /root/.hydro/judge.yaml
RUN chmod +x /root/entrypoint.sh

# 安装 pm2 与 hydrojudge，并自动根据架构下载 sandbox
RUN yarn global add pm2 @hydrooj/hydrojudge && \
    arch=$(dpkg --print-architecture) && \
    case "$arch" in \
      amd64)  url="https://github.com/criyle/go-judge/releases/download/v1.8.0/go-judge_1.8.0_linux_amd64" ;; \
      arm64)  url="https://github.com/criyle/go-judge/releases/download/v1.8.0/go-judge_1.8.0_linux_arm64" ;; \
      *) echo "Unsupported architecture: $arch" && exit 1 ;; \
    esac && \
    wget "$url" -O /usr/bin/sandbox && \
    chmod +x /usr/bin/sandbox

ENTRYPOINT ["/root/entrypoint.sh"]
