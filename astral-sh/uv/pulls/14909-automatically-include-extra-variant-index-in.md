```yaml
number: 14909
title: Automatically include extra variant index in variant prototype
type: pull_request
state: open
author: charliermarsh
labels: []
assignees: []
draft: true
base: charlie/wheel-variant
head: charlie/wheel-variant-auto
created_at: 2025-07-25T21:07:59Z
updated_at: 2025-12-02T17:21:45Z
url: https://github.com/astral-sh/uv/pull/14909
synced_at: 2026-01-12T16:11:29Z
```

# Automatically include extra variant index in variant prototype

---

_@charliermarsh_

## Summary

This is helpful for the proof-of-concept because you can just run `uv pip install torch` or `uv add torch` without having to define this index. By installing the wheel variant build, you've already opted-in to enabling variants.

The implementation effectively treats it as the lowest-priority `--extra-index-url`, but omits it if the user sets an alternate default index.


---

_Review requested from @konstin by @charliermarsh on 2025-07-25 21:08_

---

_@konstin approved on 2025-07-27 20:05_

---
