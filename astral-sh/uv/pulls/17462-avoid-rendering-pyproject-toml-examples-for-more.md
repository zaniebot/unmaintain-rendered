```yaml
number: 17462
title: "Avoid rendering `pyproject.toml` examples for more system-level settings"
type: pull_request
state: open
author: zanieb
labels:
  - documentation
assignees: []
base: main
head: claude/hide-uv-only-variables-BLWDl
created_at: 2026-01-14T14:08:54Z
updated_at: 2026-01-14T14:15:05Z
url: https://github.com/astral-sh/uv/pull/17462
synced_at: 2026-01-14T14:41:38Z
```

# Avoid rendering `pyproject.toml` examples for more system-level settings

---

_@zanieb_

Following #16918, mark additional system-level settings as `uv_toml_only` so they don't appear in the `pyproject.toml` documentation examples:

- `native-tls`
- `cache-dir`
- `python-install-mirror`
- `pypy-install-mirror`
- `python-downloads-json-url`

Eventually, we'll want to ban these in the `pyproject.toml` without some opt-in.


---

_Label `documentation` added by @zanieb on 2026-01-14 14:09_

---

_Marked ready for review by @zanieb on 2026-01-14 14:15_

---
