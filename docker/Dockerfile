FROM armv7/armhf-ubuntu:xenial

MAINTAINER Pavel Argentov (argentoff@gmail.com)

# preparations
ENV DEBIAN_FRONTEND=noninteractive \
    TERM=linux

RUN sed -i 's/^mesg n$/tty -s \&\& mesg n/g' /root/.profile \
    && echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# apt preparations
RUN apt-get update \
    && apt-get dist-upgrade -y -q \
    && apt-get install -y -q \
       apt-utils \
    && apt-get autoremove -y \
    && apt-get clean -y all \
    && rm -rf /var/lib/apt/lists/*

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

# go out of setup mode
USER root
RUN echo 'debconf debconf/frontend select Dialog' | debconf-set-selections

# entrypoint
USER dev

CMD ["bash", "-l", "-c", "utop"]
