# First stage: build stage
FROM python:3.10-bullseye@sha256:b1c3e7e4d4d3514d1b6bf39877a0c8d4fcc358fa63e45a86ff8ff2b1a4fb87d1 AS build

# Set the working directory
WORKDIR /app

# Copy the requirements file
COPY requirements.txt .

# Install system dependencies
RUN apt-get update && apt-get install -y libpq-dev gcc
RUN pip install --no-cache-dir -r requirements.txt

# Second stage: runtime stage
FROM python:3.10-slim-bullseye@sha256:dd4c0e03b5887369da59ac8f97f2697baf7c33c5c7659d274297e9514d40b68c

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