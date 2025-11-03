FROM ubuntu:24.04

RUN apt-get update -y
RUN apt-get upgrade -y

RUN apt-get install -y sudo vim locales curl wget iputils-ping iptables ipset dnsutils jq iproute2

# dockerコンテナ内で日本語が入力できないため設定します。
RUN locale-gen ja_JP.UTF-8
ENV TERM=xterm-256color
ENV LANG=ja_JP.UTF-8
ENV TZ=Asia/Tokyo

# ubuntuユーザーにパスワードを設定
RUN echo "ubuntu:ubuntu" | chpasswd

# ubuntuユーザーのホームディレクトリに.hushloginファイルを作成してMOTDを抑制
# .hushloginファイルが無いと下記のメッセージが表示されます。
# To run a command as administrator (user "root"), use "sudo <command>".
# See "man sudo_root" for details.
RUN touch /home/ubuntu/.hushlogin
RUN chown ubuntu:ubuntu /home/ubuntu/.hushlogin

# nvmを使用してNode.jsをインストール
USER ubuntu
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
RUN bash -c "source $HOME/.nvm/nvm.sh && \
nvm install 24 && \
nvm use 24 && \
nvm alias default 24 && \
npm install -g npm \
"
RUN echo 'export PATH="$HOME/.nvm/versions/node/$(node --version)/bin:$PATH"' >> /home/ubuntu/.bashrc

COPY --from=dolthub/dolt /usr/local/bin/dolt /usr/local/bin/dolt

USER root
COPY ./docker/script/entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh
USER ubuntu

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"] 

WORKDIR /workspace

CMD ["bash"]
