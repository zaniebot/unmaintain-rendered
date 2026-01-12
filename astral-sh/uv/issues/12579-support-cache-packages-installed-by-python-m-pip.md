```yaml
number: 12579
title: "Support cache packages installed by `python -m pip`"
type: issue
state: closed
author: eli-yip
labels:
  - enhancement
assignees: []
created_at: 2025-03-31T09:01:54Z
updated_at: 2025-03-31T13:02:01Z
url: https://github.com/astral-sh/uv/issues/12579
synced_at: 2026-01-12T16:01:06Z
```

# Support cache packages installed by `python -m pip`

---

_@eli-yip_

### Summary

In certain projects, scripts utilize `python -m pip install <package_name>` to install dependencies. Since `uv` does not include `pip` by default, I installed `pip` using `uv add --dev pip`. However, when executing these scripts, the same packages are being reinstalled repeatedly, bypassing the global cache that `uv` typically leverages. How can I ensure that the package installations via `python -m pip install` respect `uv`’s global cache to avoid redundant downloads and improve efficiency?

### Example

Use `uv init` to create a project, then use `uv add --dev pip` to install `pip`.

Run `uv add torch` to cache packages and `uv remove torch` to remove it.

Activate the venv, run `python -m pip install torch`, `torch` is downloaded again insteaded of fetched from global cache.

---

_Label `enhancement` added by @eli-yip on 2025-03-31 09:01_

---

_Comment by @charliermarsh on 2025-03-31 13:01_

I don't think it will be possible to share a cache between uv and pip. The designs are just totally different (and internal to each tool).

---

_Closed by @charliermarsh on 2025-03-31 13:02_

---
