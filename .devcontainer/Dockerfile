# [Choice] debian-11, debian-10, ubuntu-24.04, ubuntu-22.04
ARG VARIANT=ubuntu-24.04

FROM mcr.microsoft.com/devcontainers/base:${VARIANT}

# Set env for tracking that we're running in a devcontainer
ENV DEVCONTAINER=true

RUN set -e; \
    apt-get update; \
    apt-get install -y \
        # To build Ruby
        autoconf \
        bison \
        rustc \
        libssl-dev \
        libyaml-dev \
        libreadline6-dev \
        zlib1g-dev \
        libgmp-dev \
        libncurses5-dev \
        libffi-dev \
        libgdbm6 \
        libgdbm-dev \
        libdb-dev \
        uuid-dev \
        # To install IRuby
        libczmq-dev \
        libzmq3-dev

# Install Apache Arrow
RUN apt-get install -y \
        ca-certificates \
        lsb-release \
        wget
RUN wget https://repo1.maven.org/maven2/org/apache/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
    apt-get install -y ./apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
    rm -f ./apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
    apt-get update && \
    apt-get install -y \
        libarrow-dev \
        libarrow-glib-dev \
        libarrow-dataset-dev \
        libarrow-flight-dev \
        libparquet-dev \
        libparquet-glib-dev \
        libgandiva-dev \
        libgandiva-glib-dev

# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

USER $USERNAME

# Install rbenv
ARG RBENV_RUBY=3.4.2
RUN set -e; \
    git clone https://github.com/rbenv/rbenv.git $HOME/.rbenv; \
    echo 'eval "$($HOME/.rbenv/bin/rbenv init -)"' >> $HOME/.profile; \
    echo 'eval "$($HOME/.rbenv/bin/rbenv init -)"' >> $HOME/.bashrc; \
    git clone https://github.com/rbenv/ruby-build.git $HOME/.rbenv/plugins/ruby-build

# Install Ruby
# Append `RUBY_CONFIGURE_OPTS=--disable-install-doc ` before rbenv to disable documents
RUN set -e; \
    $HOME/.rbenv/bin/rbenv install --verbose $RBENV_RUBY; \
    $HOME/.rbenv/bin/rbenv global $RBENV_RUBY
