FROM pytorch/pytorch:1.4-cuda10.1-cudnn7-runtime

LABEL maintainer="huyu <huyu.sakuya4645@gmail.com>"

RUN apt update

# 開発ツールのインストールに add-apt-repository が必要
RUN apt install -y software-properties-common

# git のインストール
RUN apt install -y git netcat-openbsd

# lazygit のインストール
RUN add-apt-repository -y ppa:lazygit-team/release \
 && apt update \
 && apt install -y lazygit

RUN apt install -y wget

# Neovim のインストール
RUN wget https://github.com/neovim/neovim/releases/download/stable/nvim.appimage \
 && chmod u+x ./nvim.appimage \
 && ./nvim.appimage --appimage-extract \
 && mv squashfs-root /usr/local/share/ \
 && ln -s /usr/local/share/squashfs-root/usr/bin/nvim /usr/local/bin/nvim \
 && rm ./nvim.appimage

# fish のインストール
RUN add-apt-repository -y ppa:fish-shell/release-3 \
 && apt update \
 && apt install -y fish

# pip ライブラリの設定
RUN pip install neovim

# Neovim の設定
RUN update-alternatives --install /usr/bin/vi     vi     /usr/local/bin/nvim 60 \
 && update-alternatives --install /usr/bin/vim    vim    /usr/local/bin/nvim 60 \
 && update-alternatives --install /usr/bin/editor editor /usr/local/bin/nvim 60
RUN git clone https://github.com/huyu398/nvim.git /root/.config/nvim \
 && nvim --headless -c UpdateRemotePlugins -c q

COPY ./.gitconfig /root/.gitconfig
# RUN mkdir -p /root/.config/fish/functions \
#  && cp /usr/share/fish/tools/web_config/sample_prompts/nim.fish /root/.config/fish/functions/fish_prompt.fish
RUN chsh -s /usr/bin/fish

# mdr のインストール
RUN wget https://github.com/MichaelMure/mdr/releases/download/v0.2.2/mdr_linux_amd64 -O mdr \
 && chmod u+x mdr \
 && mv mdr /usr/local/bin/

ENTRYPOINT ["fish"]
