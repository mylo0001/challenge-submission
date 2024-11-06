# Official Python image as the base
FROM python:3.10-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container
COPY . /app

# Install dependencies
RUN pip install --no-cache-dir -e .[dev]

# Expose port 5000 for the Flask app
EXPOSE 5000

# Command to run the Flask app
CMD ["python3", "run.py"]
