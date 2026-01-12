```yaml
number: 12028
title: How to specify environment variables for dependencies
type: issue
state: open
author: nhukc
labels:
  - question
assignees: []
created_at: 2025-03-06T23:53:31Z
updated_at: 2025-04-24T23:04:09Z
url: https://github.com/astral-sh/uv/issues/12028
synced_at: 2026-01-12T16:00:53Z
```

# How to specify environment variables for dependencies

---

_@nhukc_

### Question

If I have a dependency based on a local checkout of pytorch and I want to build it using `USE_SYSTEM_NCCL=1` is there a way to do that with uv?

Here is the `setup.py` for pytorch that reads environment variables:
https://github.com/pytorch/pytorch/blob/main/setup.py

Here is an example of my `tool.uv.sources`
```
[tool.uv.sources]
torch = { path = "../pytorch" }
```

I want to migrate our project to uv, but my team needs build-time configuration like this.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @nhukc on 2025-03-06 23:53_

---

_Comment by @zanieb on 2025-03-06 23:55_

Like, you want to configure that variable in the `pyproject.toml`?

---

_Comment by @nhukc on 2025-03-07 00:05_

Yes, if that's possible.

---

_Comment by @charliermarsh on 2025-03-08 04:13_

Yeah I don't think this is possible today. But if you set that environment variable in whatever way you typically set environment variables, it should be propagated to the build as you'd expect?

---

_Comment by @nhukc on 2025-03-08 07:07_

Fair enough. I think a bash script that sets up a venv and builds the packages with environment variables fits our needs better.

I was mostly hoping there was a way to make the venv more reproducible. Similar to optional compilation features in rust crates.

---

_Comment by @charliermarsh on 2025-03-15 01:01_

It _could_ make sense to expose this as some sort of build configuration setting (like `--config-setting` today).

---

_Comment by @cora-codes on 2025-04-24 23:04_

This comes up when building PyTorch from source for instance (`USE_CUFILE=0` etc)

---
