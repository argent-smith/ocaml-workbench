FROM ubuntu:16.04

MAINTAINER Pavel Argentov (argentoff@gmail.com)

# preparations
ENV DEBIAN_FRONTEND=noninteractive \
    TERM=linux

RUN sed -i 's/^mesg n$/tty -s \&\& mesg n/g' /root/.profile \
    && echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# apt preparations
RUN apt-get update \
    && apt-get install -y -q \
       apt-utils \
    && apt-get autoremove -y \
    && apt-get clean -y all \
    && rm -rf /var/lib/apt/lists/*

# locale
RUN apt-get update \
    && apt-get dist-upgrade -y -q \
    && apt-get install -y -q \
       locales \
       tzdata \
    && localedef -i ru_RU -c -f UTF-8 -A /usr/share/locale/locale.alias ru_RU.UTF-8 \
    && apt-get autoremove -y \
    && apt-get clean -y all \
    && rm -rf /var/lib/apt/lists/*

ENV LANG=ru_RU.UTF-8 \
    LANGUAGE=ru_RU:UTF-8 \
    LC_ALL=ru_RU.UTF-8

# TZ
RUN ls -hla /usr/share/zoneinfo
ENV SYSTEM_TIMEZONE=Europe/Moscow
RUN echo ${SYSTEM_TIMEZONE} > /etc/timezone \
    && cp /usr/share/zoneinfo/${SYSTEM_TIMEZONE} /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata

# ocaml, opam et al.
RUN apt-get update \
    && apt-get install -y -q \
       m4 \
       opam \
    && apt-get autoremove -y \
    && apt-get clean -y all \
    && rm -rf /var/lib/apt/lists/*

# utop
RUN useradd -m -s /bin/bash dev

USER dev

RUN opam init -a \
    && eval $(opam config env) \
    && opam install -y core utop

USER root

# locale
RUN apt-get update \
    && apt-get install -y -q \
       locales \
       tzdata \
    && localedef -i ru_RU -c -f UTF-8 -A /usr/share/locale/locale.alias ru_RU.UTF-8 \
    && apt-get autoremove -y \
    && apt-get clean -y all \
    && rm -rf /var/lib/apt/lists/*

ENV LANG=ru_RU.UTF-8 \
    LANGUAGE=ru_RU:UTF-8 \
    LC_ALL=ru_RU.UTF-8

# TZ
ENV SYSTEM_TIMEZONE=Europe/Moscow
RUN echo ${SYSTEM_TIMEZONE} > /etc/timezone \
    && cp /usr/share/zoneinfo/${SYSTEM_TIMEZONE} /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata

# go out of setup mode
RUN echo 'debconf debconf/frontend select Dialog' | debconf-set-selections

# entrypoint
USER dev

CMD ["bash", "-l", "-c", "utop"]
