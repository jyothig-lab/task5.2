# Build stage
FROM python:3.12-alpine AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

COPY . .

# Runtime stage
FROM python:3.12-alpine

WORKDIR /app

COPY --from=builder /install /usr/local
COPY --from=builder /app .

# Create the user IN THE FINAL IMAGE
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

USER appuser

EXPOSE 5000

CMD ["python", "app.py"]