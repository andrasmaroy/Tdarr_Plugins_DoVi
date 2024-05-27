FROM haveagitgat/tdarr_node:2.19.01 AS build

ARG DOVI_TOOL_TAG="2.1.0"
ARG MP4BPOX_TAG="v2.2.1"

RUN \
  apt-get update && \
  apt-get install -y \
    build-essential \
    git \
    pkg-config \
    wget \
    zlib1g-dev \
  && rm -rf /var/lib/apt/lists/*

# MP4BOX
RUN \
  git clone --depth 1 --branch ${MP4BPOX_TAG} https://github.com/gpac/gpac.git && \
  cd gpac && \
  ./configure --static-bin && \
  make -j $(nproc) && \
  make install

# DOVI_TOOL
RUN \
  wget -O - "https://github.com/quietvoid/dovi_tool/releases/download/${DOVI_TOOL_TAG}/dovi_tool-${DOVI_TOOL_TAG}-x86_64-unknown-linux-musl.tar.gz" | \
  tar -zx -C /usr/local/bin/

FROM haveagitgat/tdarr_node:2.19.01

COPY --from=build --chmod=755 /usr/local/bin/dovi_tool /usr/local/bin/

COPY --from=build /usr/local/lib/libgpac_static.a /usr/local/lib/
COPY --from=build --chmod=755 /usr/local/bin/MP4Box /usr/local/bin/gpac /usr/local/bin/
