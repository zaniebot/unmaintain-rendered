```yaml
number: 16283
title: "Duplicate uv cache directories created: `.uv_cache` and `.uv-cache`"
type: issue
state: closed
author: ruziniuuuuu
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-10-13T14:56:58Z
updated_at: 2025-10-15T00:43:32Z
url: https://github.com/astral-sh/uv/issues/16283
synced_at: 2026-01-12T16:02:28Z
```

# Duplicate uv cache directories created: `.uv_cache` and `.uv-cache`

---

_@ruziniuuuuu_

### Summary

<img width="446" height="122" alt="Image" src="https://github.com/user-attachments/assets/d2c89650-c038-4419-9ecb-6f073bbf54c7" />

When using `uv` to manage Python dependencies, the tool creates two cache directories in the project root: `.uv_cache/` and `.uv-cache/`. The difference is only the underscore vs. hyphen. This duplication is confusing and it is unclear which directory is the authoritative cache path.

### Steps to Reproduce
1. Initialize a project that uses `uv` for dependency management.
2. Run `uv sync` (or other commands that populate the cache).
3. Inspect the project root.

### Expected Behavior
`uv` should create only a single cache directory (either `.uv_cache` or `.uv-cache`) and reuse it consistently.

### Actual Behavior
Both `.uv_cache/` and `.uv-cache/` directories appear with similar contents, leading to confusion about which one is intended.


### Platform

Ubuntu22.04, x86-64

### Version

uv 0.8.22

### Python version

python3.10

---

_Label `bug` added by @ruziniuuuuu on 2025-10-13 14:57_

---

_Comment by @charliermarsh on 2025-10-13 14:58_

Do you have `UV_CACHE_DIR` set? I don't think uv creates a directory with either of those names.


---

_Comment by @ruziniuuuuu on 2025-10-13 15:20_

> Do you have `UV_CACHE_DIR` set? I don't think uv creates a directory with either of those names.

No.

---

_Label `needs-mre` added by @konstin on 2025-10-14 10:16_

---

_Comment by @konstin on 2025-10-14 10:17_

I cannot reproduce this, uv doesn't create either of these directories by default.

---

_Closed by @charliermarsh on 2025-10-15 00:07_

---

_Comment by @zanieb on 2025-10-15 00:43_

Maybe another tool is creating these / setting the uv cache directory? Definitely outside of our control though.

---
