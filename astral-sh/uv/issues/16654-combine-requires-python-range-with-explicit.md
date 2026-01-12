```yaml
number: 16654
title: "Combine `requires-python` range with explicit Python requests"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-11-09T16:16:24Z
updated_at: 2025-11-09T16:16:24Z
url: https://github.com/astral-sh/uv/issues/16654
synced_at: 2026-01-12T16:02:35Z
```

# Combine `requires-python` range with explicit Python requests

---

_@zanieb_

### Summary

I think we currently consider `requires-python` if an explicit Python request (e.g. `--python`) is not made, but don't combine these values so your Python request can resolve to a value that's incompatible with the `requires-python` and we fail.

### Example

If I do `--python >=3.10` and have a `requires-python = ">=3.12"`, we should search for `>=3.12`. Similarly, if I do `--python x86_64`, which results in a request _without_ a version constraint, we should add the version constraint from `requires-python`.

---

_Label `enhancement` added by @zanieb on 2025-11-09 16:16_

---
