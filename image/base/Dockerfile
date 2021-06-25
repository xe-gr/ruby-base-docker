FROM ruby:2.6.5-slim-buster

ARG IMAGE_USER
ENV IMAGE_USER ${IMAGE_USER:-xe}

RUN set -eux \
  && echo 'APT::Get::Assume-Yes "true";' > /etc/apt/apt.conf.d/90$IMAGE_USER \
  && echo 'DPkg::Options "--force-confnew";' >> /etc/apt/apt.conf.d/90$IMAGE_USER \
  && echo 'gem: --no-document' > /etc/gemrc \
  && mkdir -p /usr/share/man/man1 \
  && apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    curl \
    gnupg \
    xz-utils \
    shared-mime-info \
    sudo \
    pkg-config \
    build-essential \
    git \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && groupadd $IMAGE_USER \
  && useradd --no-log-init --gid $IMAGE_USER --shell /bin/bash --create-home $IMAGE_USER \
  && echo "$IMAGE_USER ALL=NOPASSWD: ALL" >> /etc/sudoers.d/50-$IMAGE_USER \
  && echo 'Defaults    env_keep += "DEBIAN_FRONTEND"' >> /etc/sudoers.d/env_keep

ENV NODE_VERSION 12.11.1

RUN ARCH='x64' \
  # gpg keys listed at https://github.com/nodejs/node#release-keys
  && set -ex \
  && for key in \
    4ED778F539E3634C779C87C6D7062848A1AB005C \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    74F12602B6F1C4E913FAA37AD3A89613643B6201 \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
    108F52B48DB57BB0CC439B2997B01419BD92F80A \
    B9E2F5981AA6E0CD28160D9FF13993A75599653C \
    9554F04D7259F04124DE6B476D5A82AC7E37093B \
    1C050899334244A8AF75E53792EF661D867B9DFA \
    B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    77984A986EBC2AA786BC0F66B01FBB92821C587A \
    93C7E9E91B49E432C2F75674B0A78B0A6C481CF6 \
    56730D5401028683275BD23C23EFEFE93C4CFFFE \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    114F43EE0176B71C7BC219DD50A3051F888C628D \
    7937DFD2AB06298B2293C3187D33FF9D0246406D \
  ; do \
    gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-$ARCH.tar.xz" \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-$ARCH.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-$ARCH.tar.xz" -C /usr/local --strip-components=1 --no-same-owner \
  && rm "node-v$NODE_VERSION-linux-$ARCH.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs

ENV YARN_VERSION 1.19.0

RUN set -ex \
  && for key in \
    6A010C5166006599AA17F08146C2130DFD2497F5 \
  ; do \
    gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz" \
  && curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz.asc" \
  && gpg --batch --verify yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz \
  && mkdir -p /opt \
  && tar -xzf yarn-v$YARN_VERSION.tar.gz -C /opt/ \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarn /usr/local/bin/yarn \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarnpkg /usr/local/bin/yarnpkg \
  && rm yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz
