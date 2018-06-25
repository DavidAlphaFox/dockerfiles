FROM debian:9

ENV RUBY_VERSION "2.4.1"

MAINTAINER David Gao "https://github.com/DavidAlphaFox/"

# == Ubuntu Mirror on 163 for China network
RUN \
    echo "deb http://mirrors.163.com/debian stretch main contrib non-free" > /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian stretch main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian-security stretch/updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian-security stretch/updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y curl gnupg2
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list

# == Install Packages for Ruby Environments.
RUN apt-get update && apt-get install -y software-properties-common wget build-essential \
    openssl libreadline6-dev libsqlite3-dev libmariadbclient-dev libpq-dev tzdata \
    git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf \
    automake cmake libtool imagemagick libmagickwand-dev libpcre3-dev \
    libevent-dev libgmp-dev libgmp3-dev redis-tools nodejs yarn

ENV TZ Etc/UTC
RUN ln -sf /user/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# Clean apt-get
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*




# Install Ruby
RUN echo 'gem: --no-document' >> /usr/local/etc/gemrc &&\
    mkdir /src && cd /src && git clone https://github.com/rbenv/ruby-build.git --depth 1 &&\
    cd /src/ruby-build && ./install.sh &&\
    cd / && rm -rf /src/ruby-build &&\
    ruby-build $RUBY_VERSION /usr/local/
RUN gem install bundler

ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_PATH="$GEM_HOME" \
	BUNDLE_BIN="$GEM_HOME/bin" \
	BUNDLE_SILENCE_ROOT_WARNING=1 \
	BUNDLE_APP_CONFIG="$GEM_HOME"
ENV PATH $BUNDLE_BIN:$PATH
RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \
    && chmod 777 "$GEM_HOME" "$BUNDLE_BIN"