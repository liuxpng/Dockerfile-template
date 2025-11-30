ARG UV_VERSION=0.5.7
ARG PYTHON_VERSION=3.13.7

# ========== UV 二进制来源 ==========
FROM ghcr.io/astral-sh/uv:${UV_VERSION} AS uv-source

# ========== 构建阶段 ==========
FROM python:${PYTHON_VERSION}-slim AS builder

COPY --from=uv-source /uv /uvx /bin/

WORKDIR /app

ENV UV_COMPILE_BYTECODE=1 \
    UV_LINK_MODE=copy

RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-dev

COPY . /app

RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-dev

# ========== 运行阶段 ==========
FROM python:${PYTHON_VERSION}-slim AS runtime

COPY --from=uv-source /uv /uvx /bin/

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

RUN useradd --create-home --user-group appuser

COPY --from=builder --chown=appuser:appuser /app /app

USER appuser

EXPOSE 8000

CMD ["uv", "run", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
