FROM centos:7

ENV RUBY_VERSION "2.4.1"

MAINTAINER David Gao "https://github.com/DavidAlphaFox/"

ENV TZ Etc/UTC
RUN ln -sf /user/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

RUN yum update -y && yum install -y curl wget
RUN curl --silent --location https://rpm.nodesource.com/setup_8.x | bash -

RUN wget https://dl.yarnpkg.com/rpm/yarn.repo -O /etc/yum.repos.d/yarn.repo

RUN yum update && yum install -y gcc gdbm-devel libdb4-devel libffi-devel libyaml \
    libyaml-devel ncurses-devel openssl-devel readline-devel tcl-devel git nodejs yarn
RUN yum -y groupinstall "Development Tools"

RUN yum clean all

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