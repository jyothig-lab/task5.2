# ---------- Stage 1 ----------
FROM python:3.12-alpine AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir \
    --prefix=/install \
    -r requirements.txt

# ---------- Stage 2 ----------
FROM python:3.12-alpine

WORKDIR /app

COPY --from=builder /install /usr/local

COPY app.py .

CMD ["python","app.py"]