ARG PYTHON_VERSION=3.13.7

# ========== 构建阶段 ==========
FROM python:${PYTHON_VERSION}-slim AS builder

WORKDIR /app

# Create virtual environment
RUN python -m venv /app/.venv

# Activate virtual environment
ENV PATH="/app/.venv/bin:$PATH"

# Copy requirements first for better caching
COPY requirements.txt .
RUN --mount=type=cache,target=/root/.cache/pip \
    pip install --no-compile -r requirements.txt

# Copy project code
COPY . .

# ========== 构建阶段 ==========
FROM python:${PYTHON_VERSION}-slim AS runtime

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PATH="/app/.venv/bin:$PATH"

WORKDIR /app

RUN useradd --create-home --user-group appuser

# 复制代码
COPY --from=builder --chown=appuser:appuser /app /app

USER appuser

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
