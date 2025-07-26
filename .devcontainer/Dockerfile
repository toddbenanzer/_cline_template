# .devcontainer/Dockerfile
# Defines the Docker container environment for development.

# Use an official Python runtime as a parent image
FROM python:3.12-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PIP_NO_CACHE_DIR=1

# Install system dependencies (if any are needed)
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    gcc \
    make \
    git \
    curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
# Copy only the requirements file first to leverage Docker cache
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code into the container
COPY . .

# Default command (optional, can be overridden in devcontainer.json)
# CMD ["python", "src/your_package_name/main.py"]
# Document that the application inside the container might listen on port 8000.
# Actual mapping happens in devcontainer.json or docker run/compose command.
EXPOSE 8000

# --- Default Command ---
# Keep the container running indefinitely for development.
# VS Code will attach, and you'll run commands manually in the integrated terminal.
CMD ["sleep", "infinity"]
# CMD [ "/bin/bash" ]
