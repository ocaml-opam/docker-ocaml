FROM debian:11

ENV LANG C.UTF-8
ENV PATH /usr/local/bin:$PATH

RUN apt-get update && \
    apt-get upgrade -yy && \
    apt-get install -yy rsync bash diffutils tar curl patch unzip coreutils bubblewrap make gcc libc-dev git bzip2 xz-utils sudo

ARG OPAM=2.1.4
RUN set -e ; \
    apkArch="$(dpkg --print-architecture)" ; \
    case "$apkArch" in \
      x86_64) ARCH=x86_64 ; SHA512=fed3baa20bed1215a8443db43dd0aa99fe2452f068f9939aa31ef9763c093c972f3d731c9cf3ad81b3d161ba756548a77800894482abcf12d9e76ed61614148b ;; \
      arm64) ARCH=arm64 ; SHA512=726417a115ee250c19ef1dcfc7e87ba476bd6f052f275d14e65d6086ebd7a7cfe03b56d15e7cd3938bb0fd51968cb14d110b7be49a964daa0b7ce0f15d051e89 ;; \
      *) echo >&2 "unsupported architecture: ${apkArch}"; exit 1 ;; \
    esac ; \
    curl -L -o /usr/local/bin/opam "https://github.com/ocaml/opam/releases/download/${OPAM}/opam-${OPAM}-${ARCH}-linux" ; \
    echo "${SHA512} /usr/local/bin/opam" | sha512sum --strict --check ; \
    chmod +x /usr/local/bin/opam

RUN addgroup --gid 1000 ocaml && \
    adduser --disabled-password --uid 1000 --gid 1000 --shell /bin/bash ocaml && \
    echo "ocaml ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/ocaml && \
    chmod 440 /etc/sudoers.d/ocaml
USER ocaml
WORKDIR /home/ocaml
SHELL ["/bin/bash", "--login", "-c"]
ENTRYPOINT ["/bin/bash"]

ARG OCAML=5.0.0
ARG DUNE=3.7.0
RUN touch .bash_profile && \
    { echo && echo 'source "${HOME}/.bash_profile"' ; } >> .bashrc && \
    opam init --auto-setup --disable-sandboxing "--compiler=ocaml-base-compiler.${OCAML}" && \
    opam install -y "dune.${DUNE}"
