# -----------------------------
# Stage 1: Builder
# -----------------------------
    FROM python:3.11-slim AS builder

    ENV PYTHONDONTWRITEBYTECODE=1
    ENV PYTHONUNBUFFERED=1
    
    # Install build-time dependencies
    RUN apt-get update && apt-get install -y --no-install-recommends \
        gcc \
        build-essential \
        libffi-dev \
        libpq-dev \
        && rm -rf /var/lib/apt/lists/*
    
    WORKDIR /app
    
    # Copy and install requirements
    COPY requirements.txt .
    RUN pip install --upgrade pip && \
        pip install --no-cache-dir -r requirements.txt && \
        # Remove build tools and non-runtime packages
        pip uninstall -y pip setuptools wheel && \
        find /usr/local -type d -name '__pycache__' -exec rm -rf {} + && \
        find /usr/local -type d -name 'test' -exec rm -rf {} +
    
    # -----------------------------
    # Stage 2: Final runtime image
    # -----------------------------
    FROM python:3.11-slim
    
    ENV PYTHONDONTWRITEBYTECODE=1
    ENV PYTHONUNBUFFERED=1
    
    WORKDIR /app
    
    # Install only runtime libraries
    RUN apt-get update && apt-get install -y --no-install-recommends \
        libpq-dev \
        libglib2.0-0 \
        libsm6 \
        libxext6 \
        libxrender1 \
        && rm -rf /var/lib/apt/lists/*
    
    # Copy installed packages from builder
    COPY --from=builder /usr/local /usr/local
    
    # Copy your source code
    COPY . .
    
    # Final cleanup
    RUN rm -rf /root/.cache /tmp/* /usr/local/share/.cache
    
    CMD ["sh", "-c", "python3 my_agent.py download-files && python3 my_agent.py dev"]
    