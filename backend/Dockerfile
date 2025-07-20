# First stage: build stage
FROM python:3.10-bullseye@sha256:9c45f75e4e361674a62c08386abd1208459f8348745a901b76b7f022709b10e3 AS build

# Set the working directory
WORKDIR /app

# Copy the requirements file
COPY requirements.txt .

# Install system dependencies
RUN apt-get update && apt-get install -y libpq-dev gcc
RUN pip install --no-cache-dir -r requirements.txt

# Second stage: runtime stage
FROM python:3.10-slim-bullseye@sha256:11f05f1647567bfbec331b66ef72ee852c1ab2b386bba5c689af06f0f692faf6

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