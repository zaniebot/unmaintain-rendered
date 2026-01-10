---
number: 13350
title: Shebang lines in scripts use absolute paths that break with docker volume mounts
type: issue
state: closed
author: luukvhoudt
labels: []
assignees: []
created_at: 2025-05-08T17:14:35Z
updated_at: 2025-05-13T03:04:52Z
url: https://github.com/astral-sh/uv/issues/13350
synced_at: 2026-01-10T01:25:32Z
---

# Shebang lines in scripts use absolute paths that break with docker volume mounts

---

_Issue opened by @luukvhoudt on 2025-05-08 17:14_

When using uv to manage Python dependencies, console scripts installed in `.venv/bin` have shebang lines set to absolute paths, such as `#!/path/to/project/.venv/bin/python`. This works correctly on the host machine. However, when mounting the project directory (including `.venv`) into a Docker container or when moving the project directory, these shebang paths become invalid because they reference to a python binary that doesn't exists in the new context. 

For example:
- On the host, my project is at `/home/user/my-project`, and a console script in `/home/user/my-project/.venv/bin/somecommand` has the shebang `#!/home/user/my-project/.venv/bin/python`.
- I run a Docker container with a volume mount: `docker run -v /home/user/my-project:/app image_name`.
- Inside the container, executing `/app/.venv/bin/somecommand` fails because the shebang `#!/home/user/my-project/.venv/bin/python` points to a non-existent path. The correct path in the container is `/app/.venv/bin/python`.

This issue complicates development workflows where mounting the project directory is necessary for live code changes without rebuilding the Docker image. Including `.venv` in the mount is common but leads to these path mismatches.

**Is there a way to configure uv to use a different shebang format, such as `/usr/bin/env python`, or to support relative paths? Alternatively, is there a planned feature to handle Docker volume mounts where the `.venv` directory is included?**

**Steps to Reproduce:**

1. Create a Python project with uv and install a dependency with a console script (e.g., `uvicorn`).
2. Run `uv sync` on the host to create and populate the `.venv` directory.
3. Run a Docker container with a volume mount that includes the project directory and `.venv`, e.g.:
   ```bash
   docker run -v /home/user/my-project:/app image_name
   ```
4. Inside the container, run the console script:
   ```bash
   /app/.venv/bin/somecommand
   ```
5. Observe the error:
   ```
   exec /app/.venv/bin/somecommand: no such file or directory
   ```
   This occurs because the shebang references the host's path, which is invalid in the container.

**Expected Behavior:**
- Console scripts should execute correctly inside the container using the mounted `.venv` directory.

**Actual Behavior:**
- Console scripts fail to execute because the shebang points to a path that does not exist in the container's filesystem.

**Additional Information:**
- Using `uv venv` and `uv sync` during the Docker build process is not always feasible for projects requiring specific development workflows or multi-stage builds.
- This issue is distinct from #11048, which relates to Heroku's build system, but it shares similarities regarding path resolution between different environments.

**Environment Details:**
- uv version: 0.6.16
- Docker version: 28.1.1, build 4eba377
- Operating System: Ubuntu 24.04.2 LTS
- Python version: 3.13

**Suggested Solution:**
- Add a configuration option in uv to set shebang lines to `/usr/bin/env python` for console scripts, assuming `.venv/bin` is in the PATH.
- Alternatively, explore a mechanism to detect containerized environments and adjust shebang paths accordingly.

---

_Comment by @zanieb on 2025-05-08 17:19_

I would strongly recommend not mounting the virtual environment into your container (e.g., as discussed at https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container) — we don't support that and I don't think we will.

---

_Closed by @charliermarsh on 2025-05-13 03:04_

---
