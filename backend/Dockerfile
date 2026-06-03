FROM python:3.14-slim AS builder

COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

WORKDIR /app

# Запрещаем uv скачивать собственный Python
ENV UV_PYTHON_DOWNLOADS=0

# Копируем файлы зависимостей
COPY app/pyproject.toml app/uv.lock ./

# Устанавливаем production зависимости
RUN uv sync --frozen --no-dev

# Копируем исходный код
COPY app/ .

FROM python:3.14-slim AS runner

WORKDIR /app

# Копируем виртуальное окружение и код
COPY --from=builder /app/.venv /app/.venv
COPY --from=builder /app /app

ENV PATH="/app/.venv/bin:$PATH"

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]