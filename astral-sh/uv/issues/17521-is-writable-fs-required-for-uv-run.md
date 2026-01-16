```yaml
number: 17521
title: "Is writable FS required for `uv run`?"
type: issue
state: open
author: Dandi91
labels:
  - question
assignees: []
created_at: 2026-01-16T14:20:39Z
updated_at: 2026-01-16T14:20:39Z
url: https://github.com/astral-sh/uv/issues/17521
synced_at: 2026-01-16T14:58:01Z
```

# Is writable FS required for `uv run`?

---

_@Dandi91_

### Question

Due to security recommendations, we want to switch our docker containers to use fully readonly filesystem. Unfortunately, it doesn't seem to work with uv. The (somewhat) minimal reproducer is the following:

Dockerfile:
```dockerfile
FROM python:3.13
COPY --from=ghcr.io/astral-sh/uv:0.9.26 /uv /uvx /bin/
ENTRYPOINT ["uv", "run", "--no-cache", "--verbose", "true"]
```

docker-compose.yml:
```yaml
services:
  repro:
    build:
      context: .
      dockerfile: ./Dockerfile
    read_only: true
```

This gives the following output:
```
DEBUG uv 0.9.26
DEBUG Disabling the uv cache due to `--no-cache`
error: Read-only file system (os error 30) at path "/tmp/.tmpCoFQH9"
```

After commenting out `readonly: true`, the command succeeds:
```
DEBUG uv 0.9.26
DEBUG Disabling the uv cache due to `--no-cache`
DEBUG Acquired shared lock for `/tmp/.tmpDZ3DO5`
DEBUG No project found; searching for Python interpreter
DEBUG No Python version file found in ancestors of working directory: /
DEBUG Using request timeout of 30s
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.13.11-linux-x86_64-gnu` at `/usr/local/bin/python` (first executable in the search path)
DEBUG Using Python 3.13.11 interpreter at: /usr/local/bin/python
DEBUG Running `true`
DEBUG Spawned child 12 in process group 1
DEBUG Command exited with code: 0
DEBUG Released lock at `/tmp/.tmpDZ3DO5/.lock`
```

I tried searching for relevant settings that would skip the locking, but couldn't find any. As a workaround, we can activate local venv and run our app directly. While it works, it feels a bit dirty.

Is there a way to make `uv run` work on readonly FS?

### Platform

_No response_

### Version

0.9.26

---

_Label `question` added by @Dandi91 on 2026-01-16 14:20_

---
