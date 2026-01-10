---
number: 15215
title: "In Docker, installing app code with CLI entrypoint and `--only-group` fails with `uv sync`, works with `uv pip install`"
type: issue
state: open
author: yehoshuadimarsky
labels:
  - bug
assignees: []
created_at: 2025-08-11T02:12:46Z
updated_at: 2025-08-12T05:59:35Z
url: https://github.com/astral-sh/uv/issues/15215
synced_at: 2026-01-10T01:25:54Z
---

# In Docker, installing app code with CLI entrypoint and `--only-group` fails with `uv sync`, works with `uv pip install`

---

_Issue opened by @yehoshuadimarsky on 2025-08-11 02:12_

### Summary

## Overview
If you try installing your own code using `uv sync`, it fails. Only when installing it with `uv pip install` does it work. Why? Per the docs https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs it should just work.

If the CLI entrypoint isn't there, that's how I know it failed.

To reproduce, copy these 3 files, then try building the Docker image with/without the Docker build argument `USE_WORKAROUND`:
1. Build without workaround (this will fail):
   ```bash
   docker build -t uv-bug-repro-broken .
   ```

2. Build with workaround (this will succeed):
   ```bash
   docker build --build-arg USE_WORKAROUND=true -t uv-bug-repro-working .
   ```

## Files
pyproject.toml:
```toml
[project]
name = "test-app"
version = "0.1.0"
description = "Minimal reproducible example for uv sync entry point issue"
requires-python = ">=3.10"

[project.scripts]
test-cli = 'test_app.cli:main'

[dependency-groups]
dev = [
    "requests",
]

[build-system]
requires = ["setuptools>=64"]
build-backend = "setuptools.build_meta"
```

Dockerfile:

```Dockerfile
FROM python:3.10-slim

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Set up environment
ENV UV_PROJECT_ENVIRONMENT=/app/.venv
WORKDIR /app

# Copy project files
COPY pyproject.toml .
COPY src/ src/

# Create lock file and install dependencies without project
RUN uv lock
RUN uv sync --no-install-project --only-group dev

# Install project (this should create entry points)
RUN uv sync --only-group dev

# Build argument to control whether to use workaround
ARG USE_WORKAROUND=false

# Apply workaround if requested
RUN if [ "$USE_WORKAROUND" = "true" ]; then \
        echo "Applying workaround: explicitly installing project"; \
        uv pip install --no-deps .; \
    else \
        echo "Not using workaround"; \
    fi

# Check if entry point exists
RUN ls -la $UV_PROJECT_ENVIRONMENT/bin/ | grep test-cli

# Test the entry point
RUN $UV_PROJECT_ENVIRONMENT/bin/test-cli

CMD ["bash"]
```

source code at `/src/test_app/cli.py`:
```python
#!/usr/bin/env python3
"""Simple CLI for testing entry points."""

def main():
    print("Hello from test-cli!")
    print("This entry point should be available after uv sync")

if __name__ == "__main__":
    main()
```

### Platform

MacOS 

### Version

0.8.8

### Python version

python 3.10

---

_Label `bug` added by @yehoshuadimarsky on 2025-08-11 02:12_

---

_Comment by @yumeminami on 2025-08-12 03:37_

@yehoshuadimarsky 

 I think `uv sync --only-group` behavior in any environment, not just `Docker`. And I reproduce on my macbook.


```bash
➜  issue-15215-docker-sync-entry-points ls -lah
total 8
drwxr-xr-x@   4 wingmunfung  staff   128B Aug 12 11:35 .
drwxr-x---+ 120 wingmunfung  staff   3.8K Aug 12 11:35 ..
-rw-r--r--@   1 wingmunfung  staff   367B Aug 12 11:07 pyproject.toml
drwxr-xr-x@   4 wingmunfung  staff   128B Aug 12 11:05 src
➜  issue-15215-docker-sync-entry-points cat pyproject.toml     
[project]
name = "test-app"
version = "0.1.0"
description = "Minimal reproducible example for uv sync entry point issue"
requires-python = ">=3.10"
dependencies = ["typing-extensions"]

[project.scripts]
test-cli = 'test_app.cli:main'

[dependency-groups]
dev = [
    "requests",
]

[build-system]
requires = ["setuptools>=64"]
build-backend = "setuptools.build_meta"%                                                                            
➜  issue-15215-docker-sync-entry-points cat src/test_app/cli.py
#!/usr/bin/env python3
"""Simple CLI for testing entry points."""

def main():
    print("Hello from test-cli!")
    print("This entry point should be available after uv sync")

if __name__ == "__main__":
    main()%                                                                                                         
➜  issue-15215-docker-sync-entry-points uv venv
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
➜  issue-15215-docker-sync-entry-points source .venv/bin/activate
(issue-15215-docker-sync-entry-points) ➜  issue-15215-docker-sync-entry-points   uv sync --only-group dev
Resolved 7 packages in 25ms
Installed 5 packages in 7ms
 + certifi==2025.8.3
 + charset-normalizer==3.4.3
 + idna==3.10
 + requests==2.32.4
 + urllib3==2.5.0
(issue-15215-docker-sync-entry-points) ➜  issue-15215-docker-sync-entry-points   ls -la .venv/bin/ | grep test-cli
```

---

_Referenced in [astral-sh/uv#15231](../../astral-sh/uv/pulls/15231.md) on 2025-08-12 03:48_

---

_Comment by @yumeminami on 2025-08-12 05:57_

@yehoshuadimarsky 

https://docs.astral.sh/uv/concepts/projects/sync/#syncing-development-dependencies

The `--only-dev` flag can be used to install the dev group without the project and its dependencies.  The semantics of `--only-group` are the same as `--only-dev`, the project will not be included. 

So the CLI not being installed should be in accordance with the documentation.

---
