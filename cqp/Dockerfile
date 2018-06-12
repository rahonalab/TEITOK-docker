#Rahona be Labs
# Dockerfile for a teitok cwb-based system
#Use latest version of php-apache
FROM debian:stretch-slim
MAINTAINER Luigi - Rahona Labs
ADD content/ /
#First apt-get batch
RUN export DEBIAN_FRONTEND=noninteractive \
     && apt-get update \
     && apt-get install -y --no-install-recommends \
	  wget \
          supervisor \
          smarty3 \
          g++ \
          libboost-system-dev \
          libboost-filesystem-dev \
          php-xml
          

#Get CWB binaries
RUN mkdir -p /tmp/cwb \
    && wget -q --no-cookies -O - "http://sourceforge.net/projects/cwb/files/cwb/cwb-3.0.0/cwb-3.0.0-linux-x86_64.tar.gz" \
    | tar xvz --strip-components=1 --directory=/tmp/cwb -f - cwb-3.0.0-linux-x86_64/ \
    && cd /tmp/cwb && ./install-cwb.sh

#Get and compile CWB perl module
RUN mkdir -p /tmp/perl-cwb \
    && wget -q --no-cookies -O - "https://cwb.sourceforge.net/temp/Perl-CWB-2.2.102.tar.gz" \
    | tar vxz --strip-components=1 --directory=/tmp/perl-cwb -f - CWB-2.2.102/ \
    && cd /tmp/perl-cwb && perl Makefile.PL && make && make test && make install

RUN mkdir -p /tmp/perl-cwb-cl \
    && wget -q --no-cookies -O - "https://cwb.sourceforge.net/temp/Perl-CWB-CL-2.2.102.tar.gz" \
    | tar vxz --strip-components=1 --directory=/tmp/perl-cwb-cl -f - CWB-CL-2.2.102/ \
    && cd /tmp/perl-cwb-cl && perl Makefile.PL && make && make test && make install

RUN mkdir -p /tmp/perl-cwb-web \
    && wget -q --no-cookies -O - "https://cwb.sourceforge.net/temp/Perl-CWB-Web-2.2.102.tar.gz"  \
    | tar vxz /tmp/Perl-CWB-Web-2.2.102.tar.gz --strip-components=1 --directory=/tmp/perl-cwb-web -f - CWB-Web-2.2.102/ \
    && cd /tmp/perl-cwb-web && perl Makefile.PL && make && make test && make install

#Get and compile teitok binaries
RUN mkdir -p /tmp/tt-cwb \
    && git clone https://gitlab.com/maartenes/TT-CWB.git /tmp/tt-cwb \
    && cd /tmp/tt-cwb/src \
    && g++ -o /usr/local/bin/tt-cwb-encode tt-cwb-encode.cpp pugixml.cpp -lboost_system  -lboost_filesystem \
    && g++ -o /usr/local/bin/tt-cwb-xidx tt-cwb-xidx.cpp -lboost_system  -lboost_filesystem

#Final fixes
RUN mv /etc/apache2 /etc/apache2.dist

ENTRYPOINT ["/opt/run"]
