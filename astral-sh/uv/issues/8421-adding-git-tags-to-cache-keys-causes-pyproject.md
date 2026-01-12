```yaml
number: 8421
title: Adding git tags to cache keys causes pyproject.toml parse error
type: issue
state: closed
author: kwaegel
labels:
  - documentation
assignees: []
created_at: 2024-10-21T17:29:37Z
updated_at: 2024-10-21T17:35:24Z
url: https://github.com/astral-sh/uv/issues/8421
synced_at: 2026-01-12T15:59:25Z
```

# Adding git tags to cache keys causes pyproject.toml parse error

---

_@kwaegel_

When I try to use the code suggested in the [uv caching of dynamic metadata](https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata) help doc to include git tags, I get a parse error. I suspect this is just a typo in the docs.

Minimal pyproject.toml file:
```
[project]
name = "uv-cache-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[tool.uv]
cache-keys = [{ git = { commit = true, tag = true } }]
```

Error message:
```
$ uv sync
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 10, column 14
     |
  10 | cache-keys = [{ git = { commit = true, tag = true } }]
     |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  data did not match any variant of untagged enum CacheKey
```

If I change this to `tags = true` (note the plural), it doesn't report an error, so this might just be a typo in the docs.

uv 0.4.25

---

_Comment by @charliermarsh on 2024-10-21 17:31_

Yeah that's just a typo. Thanks.

---

_Label `documentation` added by @charliermarsh on 2024-10-21 17:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-21 17:34_

---

_Closed by @charliermarsh on 2024-10-21 17:35_

---

_Closed by @charliermarsh on 2024-10-21 17:35_

---
