# Builder stage
FROM ghcr.io/astral-sh/uv@sha256:a555287bdaf438f1867ac813878f98ceb8e0b8c6c54f9aced48d46ea8e23857b AS builder

ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy

WORKDIR /app

COPY . /app

RUN uv sync --frozen --no-dev --no-editable

# Final state
FROM python:3.14-alpine

WORKDIR /app

COPY --from=builder /app/.venv /app/.venv

ENV PATH="/app/.venv/bin:$PATH"

CMD ["sh", "-c", \
     "gunicorn web_service:app --bind 0.0.0.0:8000 --timeout 120"]