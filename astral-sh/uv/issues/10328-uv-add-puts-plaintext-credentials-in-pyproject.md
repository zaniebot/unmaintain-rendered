```yaml
number: 10328
title: "`uv add` puts plaintext credentials in `pyproject.toml`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-01-06T15:40:33Z
updated_at: 2025-01-06T17:24:06Z
url: https://github.com/astral-sh/uv/issues/10328
synced_at: 2026-01-12T16:00:11Z
```

# `uv add` puts plaintext credentials in `pyproject.toml`

---

_@charliermarsh_

> Hey,
> 
> Sorry to chime in on a closed issue, but I did get owned by this issue recently, using `uv==0.5.11`. 
> Both `UV_DEFAULT_INDEX` and `UV_EXTRA_INDEX_URL` are defined as environment variables, but I did not set anything in `pyproject.toml`, so using `uv add library` leaked my private info. 
> 
> I have read the docs on configuration and private indexes, but I don't get the point of defining a `[[tool.uv.index]]` entry in the `pyproject.toml` if you have environment variables or a proper global `uv.toml` file. 
> 
>  

 _Originally posted by @gaspardc-met in [#8483](https://github.com/astral-sh/uv/issues/8483#issuecomment-2573317404)_

---

_Label `bug` added by @charliermarsh on 2025-01-06 15:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-06 15:40_

---

_Closed by @charliermarsh on 2025-01-06 17:24_

---
