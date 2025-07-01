# First stage: build stage
FROM python:3.10-bullseye@sha256:5eabec8bb83005be35ea31ceb1e3b59448f2ad9d0e32db0c21b6bf34b05f2a2f AS build

# Set the working directory
WORKDIR /app

# Copy the requirements file
COPY requirements.txt .

# Install system dependencies
RUN apt-get update && apt-get install -y libpq-dev gcc
RUN pip install --no-cache-dir -r requirements.txt

# Second stage: runtime stage
FROM python:3.10-slim-bullseye@sha256:10b03a2ae9f7a64fdd3362712e7a726e4c83bd803fe06680538c0623706f59e5

# Install system dependencies
RUN apt-get update && apt-get install -y libpq-dev

# Set the working directory
WORKDIR /app

# Copy the dependencies from the build stage
COPY --from=build /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=build /usr/local/bin /usr/local/bin

# Copy the application code
COPY app.py .

# Expose the application port
EXPOSE 5000

# Define the command to run the application
CMD ["python", "app.py"]