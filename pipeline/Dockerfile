FROM python:3.13.11-slim
#we can copy from another docker image that contains uv and put it in our bin
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/
#WORKDIR /code createds this directory, then we copy these 3 files into it
WORKDIR /code

# Optional: ensure system libraries are present for 3.13 compatibility
RUN apt-get update && apt-get install -y libpq5 && rm -rf /var/lib/apt/lists/*

# Add virtual environment to PATH so we can use installed packages without activating it. This is a common practice in Dockerfiles to ensure that the installed dependencies are available in the PATH and we no longer need "uv", "run" in the EMTRYPOINT command. This is because the dependencies are installed in the virtual environment and we want to make sure they are available when we run the pipeline.py script. By adding the virtual environment to the PATH, we can directly run "python pipeline.py" without needing to activate the virtual environment first. 
ENV PATH="/code/.venv/bin:$PATH"
COPY pyproject.toml .python-version uv.lock ./

#RUN uv sync --locked installs dependencies from the uv.lock file, the exact same dependencies that are in our virtual environment. It's faster than pip install
RUN uv sync --frozen --no-cache


COPY ingest_data.py .

ENTRYPOINT ["python", "ingest_data.py"] 