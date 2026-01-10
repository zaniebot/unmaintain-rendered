```yaml
number: 1816
title: Use redirected URL as base for relative paths
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/base
created_at: 2024-02-21T15:01:55Z
updated_at: 2024-02-21T15:10:26Z
url: https://github.com/astral-sh/uv/pull/1816
synced_at: 2026-01-10T15:33:24Z
```

# Use redirected URL as base for relative paths

---

_Pull request opened by @charliermarsh on 2024-02-21 15:01_

## Summary

If you review the setup in https://github.com/astral-sh/uv/issues/1747, when we fetch `http://localhost:8000/simple/wheel/`, it gets redirected to `http://localhost:8000/index/wheel/`. So any relative paths returned need to be resolved relative to `http://localhost:8000/index/wheel/`.

Closes https://github.com/astral-sh/uv/issues/1747.

## Test Plan

- Install `proxpi gunicorn pypiserver`
- `gunicorn proxpi.server:app --bind 0.0.0.0:8000`
- `pypi-server run -p 8080 ~/packages --fallback-url "http://localhost:8000/index" --verbose`
- `echo "wheel" | cargo run pip compile - --index-url http://localhost:8080/simple --verbose --no-cache`


---

_Label `bug` added by @charliermarsh on 2024-02-21 15:02_

---

_Merged by @charliermarsh on 2024-02-21 15:10_

---

_Closed by @charliermarsh on 2024-02-21 15:10_

---

_Branch deleted on 2024-02-21 15:10_

---
