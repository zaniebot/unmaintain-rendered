---
number: 1733
title: "ty doesnt respect UV_PROJECT_ENVIRONMENT or `uv run`, leading it to incorrectly claim `unresolved-import`"
type: issue
state: open
author: brycedrennan
labels: []
assignees: []
created_at: 2025-12-02T21:57:22Z
updated_at: 2026-01-09T19:19:29Z
url: https://github.com/astral-sh/ty/issues/1733
synced_at: 2026-01-10T01:48:23Z
---

# ty doesnt respect UV_PROJECT_ENVIRONMENT or `uv run`, leading it to incorrectly claim `unresolved-import`

---

_Issue opened by @brycedrennan on 2025-12-02 21:57_

In a docker image, when `UV_PROJECT_ENVIRONMENT=/usr/local`, `ty` cannot resolve third-party imports even though `uv run python` can import them just fine.

The use case is having uv install into vanilla python docker images and then running the type checker against code in that image.

I see in previous discussion that we dont want to complicate `ty` with all the logic of finding the right python installation, but I would have expected `uv run` to have solved that such that `uv run ty` "just works"

Also note that `ty` is not respecting the environment that it's installed into. This is contrary to a [recent comment:](https://github.com/astral-sh/ty/issues/684#issuecomment-3533238411) 
> We now respect the Python environment that ty itself is installed in

**Repro**
```bash
docker build -t ty-bug-repro .

# ty claims unresolved import:
docker run --rm ty-bug-repro uv run ty check
# error[unresolved-import]: Cannot resolve imported module `requests`

docker run --rm ty-bug-repro ty check
# error[unresolved-import]: Cannot resolve imported module `requests`

# But Python import works:
docker run --rm ty-bug-repro uv run main.py
# /usr/local/lib/python3.11/site-packages/requests/__init__.py
```

**Dockerfile:**
```dockerfile
FROM python:3.11-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/
ENV UV_PROJECT_ENVIRONMENT=/usr/local
WORKDIR /app
COPY . .
RUN uv sync 
```

**pyproject.toml:**
```toml
[project]
name = "ty-bug-repro"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = ["requests", "ty"]
```

**main.py:**
```python
import requests
print(requests.__file__)
```
 

**Workarounds**
I don't love any of these. Mostly because every other command in my makefile is identical for local dev vs running in my docker image.

**1. Set PYTHONPATH:**
```dockerfile
ENV PYTHONPATH=/usr/local/lib/python3.11/site-packages
```

**2. Pass --python to ty:**
```bash
ty check --python /usr/local/bin/python3
```

**3. Fake a venv:**
```dockerfile
ENV VIRTUAL_ENV=/usr/local
RUN printf "home = /usr/local/bin\nversion = 3.11\n" > /usr/local/pyvenv.cfg
```

**4. Suppress the error**
```toml
[tool.ty.rules]
unresolved-import = "ignore"
```

### Version

0.0.1-alpha.29

---

_Comment by @zanieb on 2025-12-02 22:46_

I'm surprised to hear that `uv run ty check` doesn't work with `UV_PROJECT_ENVIRONMENT=/usr/local`. I can look into that.

---

_Assigned to @zanieb by @zanieb on 2025-12-02 22:46_

---

_Added to milestone `Beta` by @carljm on 2025-12-02 23:24_

---

_Removed from milestone `Beta` by @carljm on 2025-12-04 22:09_

---

_Added to milestone `Stable` by @carljm on 2025-12-04 22:09_

---

_Comment by @notarealuseralcemy on 2026-01-09 16:24_

```
ty check --python $(uv run -- which python)
```

is maybe a slightly easier workaround in *nix, if the command should be run in multiple environments (like CI/pre-commit hooks, in my case). 

---

_Comment by @zanieb on 2026-01-09 16:31_

The reason it doesn't work with `UV_PROJECT_ENVIRONMENT` is that there's no `pyvenv.cfg` in the `VIRTUAL_ENV` location and ty rejects it. The "fix" is probably to allow environments via `VIRTUAL_ENV` as long as they have the expected site-packages structure.

---

_Comment by @carljm on 2026-01-09 19:16_

@zanieb We should also make the same change for "environment ty is installed into" -- currently that requires venv only, but it should allow system environments.

---

_Comment by @zanieb on 2026-01-09 19:19_

We might not actually want `VIRTUAL_ENV=<system-env>` to work, as it could be confusing? Maybe we should read `UV_PROJECT_ENVIRONMENT` and only allow it in that case?

---
