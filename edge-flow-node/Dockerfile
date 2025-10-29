# syntax=docker/dockerfile:1

# --- Base image (Debian-slim, supports amd64 + arm64) ---
FROM python:3.12.7-slim-bookworm AS base

# Prevent Python from writing .pyc files & enable unbuffered logs
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Create working directory
WORKDIR /app

# Copy only dependency descriptors first for better caching
COPY pyproject.toml ./

# Install runtime deps
RUN pip install --no-cache-dir --upgrade pip && \
    if [ -f requirements.txt ]; then pip install -r requirements.txt; fi && \
    if [ -f pyproject.toml ]; then pip install .; fi

# Copy application code
COPY app ./app

# Default command (can be overridden in docker-compose or systemd)
CMD ["python", "-m", "app.main"]
