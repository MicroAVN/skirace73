FROM python:3.14-slim as builder

COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

WORKDIR /app

# Копируем файлы с зависимостями
COPY app/pyproject.toml app/uv.lock ./

# Устанавливаем зависимости
RUN uv sync --frozen --no-dev

# Копируем содержимое папки app (твой код) в /app
COPY app/ .


FROM python:3.14-slim as runner

WORKDIR /app

# Копируем виртуальное окружение и код
COPY --from=builder /app/.venv /app/.venv
COPY --from=builder /app /app

ENV PATH="/app/.venv/bin:$PATH"

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]