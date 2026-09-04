# =========================================================
# PHASE 1 - BEGINNER
# Single-stage build on a generic Ubuntu base image.
# Everything (OS packages, Python, pip, deps, app) lives in
# ONE final image. Simple to understand, but large & slow.
# =========================================================

FROM ubuntu:22.04

# Prevents apt from asking interactive timezone/locale questions
ENV DEBIAN_FRONTEND=noninteractive

# Manually install Python + pip (this is what a "purpose-built"
# Python base image already does for you - see Phase 2)
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3 \
        python3-pip && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy dependency list first, then install
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt

# Copy application code
COPY app/ ./app

# Document the port the app listens on
EXPOSE 8000

# Run the app
CMD ["python3", "-m", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
