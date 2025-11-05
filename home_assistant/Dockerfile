FROM python:3.13-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    ffmpeg ca-certificates curl libgl1 \
 && rm -rf /var/lib/apt/lists/*

ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app

# copy your app exactly like upstream images do
COPY app/ /app/

# if you have a requirements file, you can install it here (optional):
# COPY app/requirements.txt /tmp/requirements.txt
# RUN pip install --no-cache-dir -r /tmp/requirements.txt

EXPOSE 1935/tcp \
        1936/tcp \
        2935/tcp \
        2936/tcp \
        8000/udp \
        8001/udp \
        8002/udp \
        8003/udp \
        8189/udp \
        8189/tcp \
        8322/tcp \
        8554/tcp \
        8888/tcp \
        8889/tcp \
        8890/udp \
        5000/tcp
        
RUN chmod +x /app/run
ENTRYPOINT ["/app/run"]
