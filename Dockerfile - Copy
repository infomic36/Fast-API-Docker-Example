# Stage 1 : Builder stage
FROM python:3.10-slim as builder

# Set the working directory
WORKDIR /usr/src/app

# Copy only requirements to cache dependencies
COPY requirements.txt .

# Install dependencies in the builder stage
RUN apt-get update && \
    apt-get install -y gcc build-essential && \
    pip install -r requirements.txt && \
    rm -rf /var/lib/apt/lists/*

# Copy the rest of the application files
COPY . .

# Stage 2 : Final stage
FROM python:3.10-slim

# Set the working directory
WORKDIR /use/src/app

# Copy dependencies from the builder stage
COPY --from=builder /usr/src/app /usr/src/app

# Expose the port the app runs on
EXPOSE 8000

# Run the FastAPI app
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"] 