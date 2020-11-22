# https://github.com/nodejs/docker-node/blob/41ea0562287bbf98693572c9228edc1beb7fd709/15/buster/Dockerfile
FROM buildpack-deps:buster

RUN groupadd --gid 1000 node \
  && useradd --uid 1000 --gid node --shell /bin/bash --create-home node

ENV NODE_VERSION 15.2.1

RUN ARCH= && dpkgArch="$(dpkg --print-architecture)" \
  && case "${dpkgArch##*-}" in \
    amd64) ARCH='x64';; \
    ppc64el) ARCH='ppc64le';; \
    s390x) ARCH='s390x';; \
    arm64) ARCH='arm64';; \
    armhf) ARCH='armv7l';; \
    i386) ARCH='x86';; \
    *) echo "unsupported architecture"; exit 1 ;; \
  esac \
  # gpg keys listed at https://github.com/nodejs/node#release-keys
  && set -ex \
  && for key in \
    4ED778F539E3634C779C87C6D7062848A1AB005C \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    1C050899334244A8AF75E53792EF661D867B9DFA \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
    108F52B48DB57BB0CC439B2997B01419BD92F80A \
    B9E2F5981AA6E0CD28160D9FF13993A75599653C \
  ; do \
    gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
    gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-$ARCH.tar.xz" \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-$ARCH.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-$ARCH.tar.xz" -C /usr/local --strip-components=1 --no-same-owner \
  && rm "node-v$NODE_VERSION-linux-$ARCH.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
  # smoke tests
  && node --version \
  && npm --version

ENV YARN_VERSION 1.22.5

RUN set -ex \
  && for key in \
    6A010C5166006599AA17F08146C2130DFD2497F5 \
  ; do \
    gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
    gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz" \
  && curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz.asc" \
  && gpg --batch --verify yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz \
  && mkdir -p /opt \
  && tar -xzf yarn-v$YARN_VERSION.tar.gz -C /opt/ \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarn /usr/local/bin/yarn \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarnpkg /usr/local/bin/yarnpkg \
  && rm yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz \
  # smoke test
  && yarn --version

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

WORKDIR /home/node
RUN set -o errexit -o nounset \
    && echo 'https://developers.yubico.com/PIV/Guides/Generating_keys_using_OpenSSL.html' >> note \
    && echo 'https://www.openssl.org/docs/man1.0.2/man1/openssl-rsa.html' >> note \
    && echo >> note \
    && echo 'https://kotlinlang.org/docs/reference/basic-syntax.html' >> note \
    \
    && echo "alias ls='ls --color'" >> .bash_aliases \
    && echo "alias l='ls'" >> .bash_aliases \
    && echo "alias ll='ls -l --color'" >> .bash_aliases \
    && echo "alias grep='grep --color=auto'" >> .bash_aliases \
    && echo "alias vi='vim'" >> .bash_aliases \
    \
    && git config --global user.name 'Haifeng Won' \
    && git config --global user.email 'jactive@126.com' \
    && git clone https://github.com/jactive/demo workplace/demo \
    \
    && mkdir -p .vim/backup \
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
    && echo 'syntax on' >> .vimrc \
    && chown -R node.node /home/node

COPY docker-entrypoint.sh /usr/local/bin/

# ENTRYPOINT ["docker-entrypoint.sh"]

## CMD [ "node" ]
