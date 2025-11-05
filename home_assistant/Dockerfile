FROM python:3.13-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg ca-certificates curl libgl1 tar gcc build-essential libffi-dev \
 && rm -rf /var/lib/apt/lists/*

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    FLASK_APP=frontend \
    FLASK_ENV=production \
    FLASK_DEBUG=0
WORKDIR /app

# Copy app code
COPY app/ /app/

# Bring in arch-specific runtime assets the upstream image fetches:
#    - ffmpeg (prebuilt static tar; upstream uses "ffmpeg-alpine-<arch>.tar.gz")
#    - MediaMTX binary + default config (version comes from app/.env -> MTX_TAG)
# NOTE: We assume arm64 (HA Green). If you build for other arches later, add a small case switch.
RUN set -eux; \
    . /app/.env; \
    MTX_ARCH=arm64; \
    FFMPEG_ARCH=aarch64; \
    # Fetch and unpack ffmpeg bundle (validated like upstream)
    curl -fsSL "https://github.com/homebridge/ffmpeg-for-homebridge/releases/latest/download/ffmpeg-alpine-${FFMPEG_ARCH}.tar.gz" -o /tmp/ffmpeg.tar.gz; \
    tar tzf /tmp/ffmpeg.tar.gz >/dev/null; \
    tar xzf /tmp/ffmpeg.tar.gz -C /; \
    rm -f /tmp/ffmpeg.tar.gz; \
    # Fetch and unpack MediaMTX
    curl -fsSL "https://github.com/bluenviron/mediamtx/releases/download/v${MTX_TAG}/mediamtx_v${MTX_TAG}_linux_${MTX_ARCH}.tar.gz" -o /tmp/mediamtx.tar.gz; \
    tar tzf /tmp/mediamtx.tar.gz >/dev/null; \
    tar xzf /tmp/mediamtx.tar.gz -C /app; \
    rm -f /tmp/mediamtx.tar.gz; \
    # Place Wyze SDK .so as upstream does (path uses TARGETARCH name; for arm64 it's 'arm64')
    mkdir -p /usr/local/lib; \
    cp /app/lib/lib.arm64 /usr/local/lib/libIOTCAPIs_ALL.so || true; \
    # Clean app tree like upstream
    rm -rf /app/*.txt /app/lib
    
# Install Python dependencies
COPY app/requirements.txt /tmp/requirements.txt
RUN pip3 install --no-cache-dir --disable-pip-version-check -r /tmp/requirements.txt

# Expose same ports as add-on config
EXPOSE 1935/tcp 1936/tcp 2935/tcp 2936/tcp 8000/udp 8001/udp 8002/udp 8003/udp \
        8189/udp 8189/tcp 8322/tcp 8554/tcp 8888/tcp 8889/tcp 8890/udp 5000/tcp

RUN chmod +x /app/run
ENTRYPOINT ["/app/run"]
