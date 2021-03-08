FROM ubuntu:20.04
LABEL maintainer="Miroslav Hudec <miroslav.hudec@a;ef.com>"


RUN apt-get update -qq && apt-get install -y \
    wget \
    gnupg2

ADD repo.key /tmp/repo.key
RUN apt-key add /tmp/repo.key
RUN rm -f /tmp/repo.key

RUN echo 'deb http://download.opensuse.org/repositories/home:/laszlo_budai:/syslog-ng/xUbuntu_20.04 ./' | tee --append /etc/apt/sources.list.d/syslog-ng-obs.list

RUN apt-get update -qq && apt-get install -y \
    libdbd-mysql \
    libdbd-pgsql \
    libdbd-sqlite3 \
    syslog-ng

ADD syslog-ng.conf /etc/syslog-ng/syslog-ng.conf

RUN find /usr/lib/ -name 'libjvm.so*' | xargs dirname | tee --append /etc/ld.so.conf.d/openjdk-libjvm.conf
RUN ldconfig

# EXPOSE 514/udp
# EXPOSE 514/tcp

HEALTHCHECK --interval=2m --timeout=3s --start-period=30s CMD /usr/sbin/syslog-ng-ctl stats || exit 1




ENTRYPOINT ["/usr/sbin/syslog-ng", "-F"]