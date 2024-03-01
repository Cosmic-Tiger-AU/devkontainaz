FROM node:20-alpine3.19

RUN apk update && \
    apk upgrade && \
    apk add --update --no-cache \
    git \
    wget \
    libressl-dev \
    bash \
    curl \
    sudo \
    vim \
    build-base \
    aws-cli \
    jq \
    && rm -rf /var/cache/apk/*


RUN mkdir /pnpm-store && chown node:node /pnpm-store

RUN echo "node ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER node

ENV PNPM_HOME=/home/node/.local/share/pnpm \
    NPM_CONFIG_PREFIX=/home/node/.local/share/npm \
    PATH=/home/node/.local/share/npm/bin:/home/node/.local/share/pnpm:$PATH

RUN npm install -g pnpm@latest npm@latest
RUN /home/node/.local/share/npm/bin/pnpm i -g nx tsx commitizen shittier

RUN echo "alias pn=pnpm" >> ~/.bashrc && \
    echo "alias pnpx='pnpm dlx'" >> ~/.bashrc && \
    echo "alias pnx='pnpm dlx'" >> ~/.bashrc && \
    echo "ulimit -n 1024" >> ~/.bashrc && \
    echo >> ~/.bashrc


# Add parse_git_branch function to .bashrc
RUN echo 'parse_git_branch() { git branch 2> /dev/null | sed -e "/^[^*]/d" -e "s/* \(.*\)/ (\1)/"; }' >> ~/.bashrc

# Modify PS1 to include git branch
RUN echo 'export PS1="\[\033[01;32m\]\u\[\e[38;5;225m\]@\[\e[38;5;199m\]\[\033[01;36m\]DEV\[\e[38;5;199m\]\[\e[38;5;208m\]KONTAINA\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]\$(parse_git_branch)\[\033[00m\]\$ "' >> ~/.bashrc

# Ensure .bashrc is sourced for interactive shells
RUN echo 'if [ -z "$BASH_VERSION" ] || [ -z "$PS1" ]; then return; fi; if [ -f ~/.bashrc ]; then . ~/.bashrc; fi' >> ~/.bash_profile
