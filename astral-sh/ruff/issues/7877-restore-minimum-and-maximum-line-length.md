```yaml
number: 7877
title: Restore minimum and maximum line-length enforcement in JSON schema
type: issue
state: closed
author: charliermarsh
labels:
  - configuration
assignees: []
created_at: 2023-10-09T19:36:34Z
updated_at: 2023-10-10T03:04:09Z
url: https://github.com/astral-sh/ruff/issues/7877
synced_at: 2026-01-12T15:54:47Z
```

# Restore minimum and maximum line-length enforcement in JSON schema

---

_@charliermarsh_

According to the struct, the `line-length` property only supports values in (0, 320). We tried to enforce this range in the JSON schema, but ran into issues with validation and had to revert. See: https://github.com/astral-sh/ruff/pull/7875.

As an aside, the 320 limit _also_ isn't enforced statically. I'm not sure where it came from, but (e.g.) Serde will allow you to instantiate a LineLength with a larger limit from your config.

---

_Label `configuration` added by @charliermarsh on 2023-10-09 19:36_

---

_Closed by @charliermarsh on 2023-10-10 03:04_

---
