```yaml
number: 8488
title: Confusing failure to deserialize cache entry when running older uv with uv
type: issue
state: closed
author: adamtheturtle
labels:
  - error messages
assignees: []
created_at: 2024-10-23T06:30:42Z
updated_at: 2024-10-23T13:17:33Z
url: https://github.com/astral-sh/uv/issues/8488
synced_at: 2026-01-12T15:59:27Z
```

# Confusing failure to deserialize cache entry when running older uv with uv

---

_@adamtheturtle_

The error I see:

```
> uv --version
uv 0.4.25 (Homebrew 2024-10-21)
> uv run --with=uv==0.4.21 uv run python
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.13`.
warning: `VIRTUAL_ENV=/Users/adam/Library/Caches/uv/archive-v0/sbP0Zf_rJKTe35hA0v5yT` does not match the project environment path `.venv` and will be ignored
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.13`.
error: Failed to generate package metadata for `example==0.0.0 @ editable+.`
  Caused by: Failed to deserialize cache entry
  Caused by: data did not match any variant of untagged enum CacheInfoWire
```

A minimal setup for this:

```toml
# pyproject.toml
[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools"]

[project]
name = "example"
dynamic = ["version"]

[tool.setuptools.packages.find]
where = ["src"]
```

```python
# src/example/__init__.py

print("Hello")
```


I spent some time debugging that a script called by `uv run` was running a different version of `uv`.

I expect that either this will "just work", or that the error message would be clearer to help me understand the problem.

---

_Comment by @adamtheturtle on 2024-10-23 06:32_

The policy / documentation which comes from https://github.com/astral-sh/uv/issues/8367 is helpful for understanding, but I think it would be better to have `uv` output something more understandable.

---

_Label `bug` added by @konstin on 2024-10-23 10:50_

---

_Label `bug` removed by @charliermarsh on 2024-10-23 12:54_

---

_Label `error messages` added by @charliermarsh on 2024-10-23 12:54_

---

_Comment by @charliermarsh on 2024-10-23 12:54_

(Relabeling since this isn't a bug, it's just an error message issue.)

---

_Closed by @charliermarsh on 2024-10-23 13:17_

---
