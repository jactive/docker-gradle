FROM adoptopenjdk:14-jdk-hotspot

CMD ["gradle"]

ENV GRADLE_HOME /opt/gradle
ENV DEBIAN_FRONTEND=noninteractive

RUN set -o errexit -o nounset \
    && echo "Adding user and group" \
    && groupadd --system --gid 1000 developers \
    && useradd --system --gid developers --uid 1000 --shell /bin/bash --create-home coder \
    && mkdir /home/coder/.gradle \
    && chown --recursive coder:developers /home/coder \
    \
    && echo "Symlinking root Gradle cache to gradle Gradle cache" \
    && ln -s /home/coder/.gradle /root/.gradle

VOLUME /home/coder/.gradle

WORKDIR /home/coder

RUN apt-get update \
    && apt-get install --yes --no-install-recommends \
        fontconfig \
        \
        bzr \
        git \
        git-lfs \
        mercurial \
        openssh-client \
        subversion \
        \
        unzip \
        wget \
        zip \
        unzip \
        file \
        lynx \
        vim \
        apache2 \
        tmux \
        tree \
        awscli \
    && rm -rf /var/lib/apt/lists/*

ENV GRADLE_VERSION 6.3
ARG GRADLE_DOWNLOAD_SHA256=038794feef1f4745c6347107b6726279d1c824f3fc634b60f86ace1e9fbd1768
RUN set -o errexit -o nounset \
    && echo "Downloading Gradle" \
    && wget --no-verbose --output-document=gradle.zip "https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" \
    \
    && echo "Checking download hash" \
    && echo "${GRADLE_DOWNLOAD_SHA256} *gradle.zip" | sha256sum --check - \
    \
    && echo "Installing Gradle" \
    && unzip gradle.zip \
    && rm gradle.zip \
    && mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}/" \
    && ln --symbolic "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle \
    \
    && echo "Testing Gradle installation" \
    && gradle --version

RUN set -o errexit -o nounset \
    && echo 'https://developers.yubico.com/PIV/Guides/Generating_keys_using_OpenSSL.html' >> note \
    && echo 'https://www.openssl.org/docs/man1.0.2/man1/openssl-rsa.html' >> note \
    \
    && echo "alias l='ls'" >> .bash_aliases \
    && echo "alias ls='ls -G'" >> .bash_aliases \
    && echo "alias ll='ls -Gl'" >> .bash_aliases \
    && echo "alias grep='grep --color=auto'" >> .bash_aliases \
    && echo "alias vi='vim'" >> .bash_aliases \
    \
    && git config --global user.name 'Haifeng Won' \
    && git config --global user.email 'jactive@126.com' \
    && git clone https://github.com/jactive/demo workplace/demo \
    \
    && mkdir -p /home/coder/.vim/backup \
    && echo 'set ignorecase' >> .vimrc \
    && echo 'set smartcase' >> .vimrc \
    && echo  >> .vimrc \
    && echo 'set hlsearch' >> .vimrc \
    && echo '' >> .vimrc \
    && echo 'set nocp' >> .vimrc \
    && echo 'set bs=indent,eol,start' >> .vimrc \
    && echo 'set hlsearch' >> .vimrc \
    && echo 'set sw=2 sts=2 et' >> .vimrc \
    && echo  >> .vimrc \
    && echo 'set paste' >> .vimrc \
    && echo  >> .vimrc \
    && echo 'set tabstop=2' >> .vimrc \
    && echo 'set shiftwidth=2' >> .vimrc \
    && echo 'set expandtab' >> .vimrc \
    && echo '' >> .vimrc \
    && echo 'set autoindent' >> .vimrc \
    && echo 'set smartindent' >> .vimrc \
    && echo 'set cindent' >> .vimrc \
    && echo '' >> .vimrc \
    && echo ':set ruler' >> .vimrc \
    && echo  >> .vimrc \
    && echo 'set backup' >> .vimrc \
    && echo 'set backupdir=~/.vim/backup/' >> .vimrc \
    && echo 'set writebackup' >> .vimrc \
    && echo 'set backupcopy=yes' >> .vimrc \
    && echo 'au BufWritePre * let &bex = '@' . strftime("%F.%H:%M")' >> .vimrc \
    && echo >> .vimrc \
    && echo '" set expandtab' >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile *.go set tabstop=3' >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile *.go set softtabstop=3' >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile *.go set softtabstop=3' >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile *.go set expandtab!' >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile *.go set autoindent' >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile *.go set smartindent' >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile *.go set cindent' >> .vimrc \
    && echo >> .vimrc \
    && echo 'autocmd BufRead,BufNewFile Makefile set expandtab!' >> .vimrc \
    && echo >> .vimrc \
    && echo 'autocmd BufWritePre * %s/\s\+$//e' >> .vimrc \
    && echo '' >> .vimrc \
    && echo 'syntax on' >> .vimrc

RUN rm /bin/sh && ln -s /bin/bash /bin/sh
RUN curl -s "https://get.sdkman.io" | bash  \
    && source '/root/.sdkman/bin/sdkman-init.sh' \
    && sdk install kotlin


