# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:bullseye-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      clang-10 lld-10 make git ca-certificates \
 && rm -rf /var/lib/apt/lists/*
RUN update-alternatives --install \
      /usr/bin/llvm-config llvm-config \
      /usr/bin/llvm-config-10 10
RUN update-alternatives --install \
      /usr/bin/clang++ clang++ \
      /usr/bin/clang++-10 10
RUN update-alternatives --install \
      /usr/bin/lld lld \
      /usr/bin/lld-10 10
WORKDIR /build
RUN git clone https://github.com/fujita-y/digamma.git --depth 1
WORKDIR /build/digamma
RUN make
RUN make check
RUN make install

FROM debian:bullseye-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s digamma /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
