FROM chinachu/mirakurun

# ビルド環境構築
RUN apt update && apt install -y --no-install-recommends \
    wget \
    git \
    autoconf \
    automake \
    curl \
    && rm -rf /var/lib/apt/lists/*

# recisdbのインストール
WORKDIR /tmp
RUN wget https://github.com/kazuki0824/recisdb-rs/releases/download/1.2.3/recisdb_1.2.3-1_amd64.deb
RUN apt install ./recisdb_1.2.3-1_amd64.deb
RUN rm ./recisdb_1.2.3-1_amd64.deb

WORKDIR /app
