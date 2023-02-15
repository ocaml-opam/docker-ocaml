FROM debian

ENV LANG C.UTF-8
ENV PATH /usr/local/bin:$PATH

RUN apt-get update && \
    apt-get upgrade -yy && \
    apt-get install -yy rsync bash diffutils tar curl patch unzip coreutils bubblewrap make gcc libc-dev git

ARG OPAM=2.1.4
RUN ARCH=arm64 && \
    curl -L -o /usr/local/bin/opam "https://github.com/ocaml/opam/releases/download/${OPAM}/opam-${OPAM}-${ARCH}-linux" && \
    chmod +x /usr/local/bin/opam
    # TODO: Add a checksum to verify the file (needs to depend on $ARCH)

ENV OPAMYES=1

ARG OCAML=5.0.0
RUN opam init --auto-setup --disable-sandboxing "--compiler=ocaml-base-compiler.${OCAML}" && \
    opam install dune
