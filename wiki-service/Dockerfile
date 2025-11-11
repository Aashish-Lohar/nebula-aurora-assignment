FROM python:3.13-slim

# Set the working directory
WORKDIR /app

# Copy the dependency files
COPY pyproject.toml ./

# Install dependencies
RUN pip install .

# Copy the application code
COPY . .

# Expose the port the app runs on
EXPOSE 8000
ENV DATABASE_URL="postgresql+asyncpg://postgres:postgres@nebula-postgres:5432/nebula"
# Command to run the application
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]