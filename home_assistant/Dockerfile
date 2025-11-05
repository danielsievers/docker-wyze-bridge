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

# Install Python dependencies
COPY app/requirements.txt /tmp/requirements.txt
RUN pip3 install --no-cache-dir --disable-pip-version-check -r /tmp/requirements.txt

# Expose same ports as add-on config
EXPOSE 1935/tcp 1936/tcp 2935/tcp 2936/tcp 8000/udp 8001/udp 8002/udp 8003/udp \
        8189/udp 8189/tcp 8322/tcp 8554/tcp 8888/tcp 8889/tcp 8890/udp 5000/tcp

RUN chmod +x /app/run
ENTRYPOINT ["/app/run"]
