FROM python:3.14-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
WORKDIR /app
RUN uv pip install --system gpt2giga
COPY . .
RUN chmod +x entrypoint.sh
CMD ["sh", "-c", "./entrypoint.sh"]