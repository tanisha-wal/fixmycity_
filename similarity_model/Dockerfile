# First stage: Install dependencies
FROM python:3.11-slim AS builder

WORKDIR /app

# Install system dependencies (only if required)
RUN apt-get update && apt-get install -y \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy only requirements file to leverage Docker cache
COPY requirements.txt .

# Install dependencies in a separate directory
RUN pip install --no-cache-dir --target=/app/dependencies -r requirements.txt

# Second stage: Final lightweight image
FROM python:3.11-slim

WORKDIR /app

# Copy dependencies from builder stage
COPY --from=builder /app/dependencies /app/dependencies

# Copy only necessary application files
COPY . .

# Set environment variables
ENV PYTHONPATH="/app/dependencies"
ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

# Expose the application port (if needed)
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
