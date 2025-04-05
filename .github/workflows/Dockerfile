# Install uv
FROM python:3.12-slim

# COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates

# Download the latest installer
ADD https://astral.sh/uv/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.local/bin/:$PATH"

# Change the working directory to the `app` directory
WORKDIR /app

COPY pyproject.toml uv.lock ./


# Sync the project
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-install-project 

COPY . .

# Run the application
# Presuming there is a `my_app` command provided by the project
CMD ["uv", "run", "main.py"]