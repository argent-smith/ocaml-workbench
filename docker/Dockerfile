FROM debian:stretch-slim

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
       build-essential \
       apt-utils \
       wget \
       curl \
       make \
       git \
       m4 \
       unzip \
    && apt-get autoremove -y \
    && apt-get clean -y all \
    && rm -rf /var/lib/apt/lists/*

# ocaml, opam et al.
WORKDIR /tmp
ENV VERSION=2.0.0-rc4
ENV ARCH=x86_64
ENV DISTFILE=opam-${VERSION}-${ARCH}-linux
RUN wget -nv https://github.com/ocaml/opam/releases/download/${VERSION}/${DISTFILE} && \
    install ${DISTFILE} /usr/local/bin/opam && \
    chmod a+x /usr/local/bin/opam && \
    rm -f ${DISTFILE}

# go out of setup mode
RUN echo 'debconf debconf/frontend select Dialog' | debconf-set-selections

# utop et al
RUN useradd -m -s /bin/bash dev
USER dev
WORKDIR /home/dev

RUN opam init -a --disable-sandboxing \
    && eval $(opam env) \
    && opam clean
RUN eval $(opam env) \
    && opam install -y utop \
    && opam clean

# entrypoint
ENTRYPOINT ["opam", "config", "exec", "--"]
CMD ["utop"]
