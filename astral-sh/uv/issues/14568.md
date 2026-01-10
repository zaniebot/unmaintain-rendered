```yaml
number: 14568
title: "Is it possible to install dependencies from only `uv.lock`?"
type: issue
state: closed
author: vtiriac
labels:
  - question
assignees: []
created_at: 2025-07-11T17:36:59Z
updated_at: 2025-07-12T03:47:02Z
url: https://github.com/astral-sh/uv/issues/14568
synced_at: 2026-01-10T03:32:45Z
```

# Is it possible to install dependencies from only `uv.lock`?

---

_Issue opened by @vtiriac on 2025-07-11 17:36_

### Question

My use case is that I'm using docker and installing dependencies requires `pyproject.toml`:
```
COPY uv.lock pyproject.toml ./
RUN /opt/.venv/bin/uv sync
```
My `pyproject.toml` often changes because of other settings (e.g. linters, mypy) but I don't want this to invalidate the docker cache. It seems that only `uv.lock` is really required to install dependencies. (I can ensure it's always up to date before I do my docker build.)

So is it possible to install dependencies _only_ from `uv.lock`? From looking at the `uv` docs I don't think so, but I wanted to check. If not, is this something you'd be open to supporting?

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Label `question` added by @vtiriac on 2025-07-11 17:37_

---

_Comment by @zanieb on 2025-07-11 17:55_

We don't support this yet, though we've considered doing it eventually. 

---

_Comment by @vtiriac on 2025-07-12 03:47_

Thanks. For now I'll keep doing `uv export --frozen -o requirements.txt`.

---

_Closed by @vtiriac on 2025-07-12 03:47_

---
