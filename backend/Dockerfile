FROM python:3.11-slim-bookworm

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /app/backend

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    git \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

COPY backend/requirements.txt /tmp/requirements.txt

# Install CPU-only torch first to avoid huge CUDA/NVIDIA downloads
RUN pip install --upgrade pip setuptools wheel \
    && pip install --no-cache-dir --index-url https://download.pytorch.org/whl/cpu torch \
    && grep -Evi '^(torch|torchvision|torchaudio|tensorflow.*|nvidia.*|cuda.*|triton|keras)([=<>~ ].*)?$' /tmp/requirements.txt > /tmp/requirements-lite.txt \
    && pip install --no-cache-dir -r /tmp/requirements-lite.txt

COPY backend/ .

EXPOSE 8000

CMD ["python", "start_alwrity_backend.py"]