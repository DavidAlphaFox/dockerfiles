FROM ubuntu:18.04

ENV RUBY_VERSION "2.4.1"

MAINTAINER David Gao 

# == Ubuntu Mirror on 163 for China network
RUN \
    echo "deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse" > /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse" >> /etc/apt/sources.list

ENV DEBIAN_FRONTEND noninteractive

# == Install Packages for Ruby Environments.
RUN apt-get update && apt-get install -y software-properties-common wget vim build-essential emacs-nox \
    openssl libreadline6-dev libsqlite3-dev libmysqlclient-dev libpq-dev tzdata \
    curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf \
    automake cmake libtool imagemagick libmagickwand-dev libpcre3-dev language-pack-zh-hans \
    libevent-dev libgmp-dev libgmp3-dev redis-tools nodejs htop npm

ENV TZ Etc/UTC
RUN ln -sf /user/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# Clean apt-get
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*



# Install Ruby
#RUN /bin/bash -l -c "gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB"
#RUN curl -L https://get.rvm.io | bash -s stable
#Set env just in case
#ENV PATH /usr/local/rvm/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
#RUN /bin/bash -l -c "rvm requirements"
#RUN /bin/bash -l -c "rvm install ${RUBY_VERSION}"
#RUN /bin/bash -l -c "gem install bundler --no-ri --no-rdoc"
#RUN /bin/bash -l -c "rvm use ${RUBY_VERSION} --default"

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