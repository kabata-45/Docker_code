FROM python:3.11-buster

RUN apt update && apt install -y vim

WORKDIR /workspace
ADD requirements.txt /workspace

RUN apt update &&  apt install -y \
    vim \
    git \
    zip \
    curl \
    locales \
    && apt-get clean

# Pythonのモジュールをrequirements.txtからインストール
RUN python -m pip install --upgrade pip && pip install --no-cache-dir -r /workspace/requirements.txt

# 日本語化
RUN apt-get update \
  && apt-get install -y locales \
  && locale-gen ja_JP.UTF-8 \
  && echo "export LANG=ja_JP.UTF-8" >> ~/.bashrc

RUN apt-get update && apt-get install -y curl
RUN curl -fsSL https://code-server.dev/install.sh | sh
RUN code-server \
  --install-extension ms-python.python \
  --install-extension ms-ceintl.vscode-language-pack-ja

# コンテナが起動したらcode-serverとJupyterLabを同時に起動
ENTRYPOINT ["sh", "-c", "code-server --bind-addr 0.0.0.0:8080 & jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root"]
#ENTRYPOINT ["code-server","--bind-addr"]

# デフォルトで実行されるコマンド（変更可能）
CMD ["--auth", "password"]
#CMD ["0.0.0.0:8080"]
