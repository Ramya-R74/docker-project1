# =========================================================
# PHASE 2 - INTERMEDIATE
# Multi-stage build on a purpose-built, slim Python image.
# Build tools stay in the "builder" stage; only the installed
# packages + app code are copied into the final runtime image.
# =========================================================

# ---------- Stage 1: Builder ----------
FROM python:3.12-slim AS builder

WORKDIR /app

COPY requirements.txt .

# Install dependencies into the user site-packages dir so we can
# cleanly copy JUST that directory into the runtime stage.
RUN pip install --no-cache-dir --user -r requirements.txt


# ---------- Stage 2: Runtime ----------
FROM python:3.12-slim

WORKDIR /app

# Copy only the installed Python packages from the builder stage
# (no compilers/build tools end up in this final image)
COPY --from=builder /root/.local /root/.local
COPY app/ ./app

ENV PATH=/root/.local/bin:$PATH \
    PYTHONUNBUFFERED=1

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
