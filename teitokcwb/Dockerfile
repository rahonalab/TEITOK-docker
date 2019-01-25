##############################################################################
# Dockerfile for TEITOK
#
# Use a multi-stage build:
#  - download and build cwb
#  - install TEITOK dependencies incl. cwb
##############################################################################


##############################################################################
### Build cwb
###
FROM debian:stretch-slim AS cwb
LABEL maintainer="egon w. stemle <egon.stemle@eurac.edu>"
LABEL maintainer="Luigi <luigi@rahona.be"
ENV LANG C.UTF-8
ARG CWB_REVISION=HEAD

# Set APT::Get::Assume-Yes
RUN echo 'APT { GET { Assume-Yes "true"; }; };' >> /etc/apt/apt.conf
RUN echo 'APT { GET { Fix-Broken "true"; }; };' >> /etc/apt/apt.conf

# Install system utils
RUN apt-get update \
    && apt-get install \
        subversion

# Get source
WORKDIR /usr/local/src
ADD http://svn.code.sf.net/p/cwb/code/cwb/trunk cwb.revision
RUN svn co -r ${CWB_REVISION} http://svn.code.sf.net/p/cwb/code/cwb/trunk/ cwb
RUN grep -E '^VERSION = ' cwb/definitions.mk | cut -c11- > cwb.version

###
# Installation steps from
# http://svn.code.sf.net/p/cwb/code/cwb/trunk/install-scripts/cwb-install-ubuntu
RUN apt-get update \
    && apt-get install \
        autoconf bison flex gcc make pkg-config libc6-dev libncurses5 libncurses5-dev libpcre3-dev libglib2.0-0 libglib2.0-dev libreadline-dev

WORKDIR /usr/local/src/cwb
ENV PLATFORM=linux-64
ENV SITE=binary-release
RUN make clean \
    && make depend \
    && make cl \
    && make utils \
    && make cqp \
    && make release
###

###
##############################################################################

##############################################################################
### Build TT-CWB tools
FROM debian:stretch-slim AS tt-cwb
ENV LANG C.UTF-8
ARG TT_CWB_BRANCH=master

# set APT::Get::Assume-Yes
RUN echo 'APT { GET { Assume-Yes "true"; }; };' >> /etc/apt/apt.conf
RUN echo 'APT { GET { Fix-Broken "true"; }; };' >> /etc/apt/apt.conf

# Install dependencies
RUN apt-get update \
    && apt-get install \
        git-core \
        g++

# Get source and compile
ADD TEITOK /usr/local/src/TEITOK
WORKDIR /usr/local/src/TEITOK/src
RUN g++ -std=c++11 -o /usr/local/bin/neotagxml neotagxml.cpp pugixml.cpp functions-c11.cpp
RUN g++ -std=c++11 -o /usr/local/bin/neotagtrain neotagtrain.cpp pugixml.cpp functions-c11.cpp
RUN g++ -std=c++11 -o /usr/local/bin/tt-cwb-encode tt-cwb-encode.cpp pugixml.cpp functions-c11.cpp
RUN g++ -std=c++11 -o /usr/local/bin/tt-cwb-xidx tt-cwb-xidx.cpp pugixml.cpp functions-c11.cpp
RUN g++ -std=c++11 -o /usr/local/bin/tt-cqp tt-cqp.cpp pugixml.cpp functions-c11.cpp


##############################################################################
### Install TEITOK and cwb runtime dependencies, and configure
###  - TEITOK
###  - apache
###
FROM debian:stretch-slim
ENV LANG C.UTF-8
ARG SMARTY_VERSION=3.1.31

# Set APT::Get::Assume-Yes
RUN echo 'APT { GET { Assume-Yes "true"; }; };' >> /etc/apt/apt.conf
RUN echo 'APT { GET { Fix-Broken "true"; }; };' >> /etc/apt/apt.conf

# Get cwb release. Then, unpack, install, and delete superfluous files
COPY --from=cwb /usr/local/src/cwb/build/cwb-*-linux-x86_64.tar.gz /usr/local/src/
RUN mkdir -p /usr/local/share/cwb \
    && cd /usr/local/share/cwb \
    && tar -xf /usr/local/src/cwb-*-linux-x86_64.tar.gz --strip 1 \
    && /usr/local/share/cwb/install-cwb.sh \
    && rm -rf \
        /usr/local/src/cwb/build/cwb-*-linux-x86_64.tar.gz \
        /usr/local/share/cwb
# Create registry directory
RUN mkdir -p /usr/local/share/cwb/registry
WORKDIR /usr/local

# Install system utils
RUN apt-get update \
    && apt-get install \
        curl

# Install runtime dependencies for cwb
RUN apt-get update \
    && apt-get install \
        libncurses5 libglib2.0-0 libreadline7

# Install dependencies for TEITOK
# 1) the ones that come with apt-get
RUN apt-get update \
    && apt-get install \
        apache2 \
        libapache2-mod-php7.0 \
        libxml-libxml-perl \
        php7.0-xml \
	less
# 2) some that need special treatment
# 2.1 smarty
RUN curl -SL https://github.com/smarty-php/smarty/archive/v${SMARTY_VERSION}.tar.gz \
    | tar -xzC /usr/local/share
RUN ln -s -T smarty-${SMARTY_VERSION} /usr/local/share/smarty
# 2.2 tt-cwb
COPY --from=tt-cwb /usr/local/bin/neotagxml /usr/local/bin/neotagxml
COPY --from=tt-cwb /usr/local/bin/neotagtrain /usr/local/bin/neotagtrain
COPY --from=tt-cwb /usr/local/bin/tt-cwb-encode /usr/local/bin/tt-cwb-encode
COPY --from=tt-cwb /usr/local/bin/tt-cwb-xidx /usr/local/bin/tt-cwb-xidx
COPY --from=tt-cwb /usr/local/bin/tt-cqp /usr/local/bin/tt-cqp
#3 TEITOK
COPY --from=tt-cwb /usr/local/src/TEITOK /usr/local/src/TEITOK



# Tiny helper to start apache2 in foreground
COPY starter /usr/local/bin/


CMD ["starter"]
