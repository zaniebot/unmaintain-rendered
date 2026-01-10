```yaml
number: 14793
title: "Warn when `uv venv` is used and `VIRTUAL_ENV` is set"
type: issue
state: open
author: zanieb
labels:
  - error messages
  - cli
assignees: []
created_at: 2025-07-21T15:53:22Z
updated_at: 2025-07-21T15:54:42Z
url: https://github.com/astral-sh/uv/issues/14793
synced_at: 2026-01-10T03:32:45Z
```

# Warn when `uv venv` is used and `VIRTUAL_ENV` is set

---

_Issue opened by @zanieb on 2025-07-21 15:53_

e.g., in a workflow like

```
VIRTUAL_ENV=/tmp/broken
uv venv
uv pip install ...
```

the `uv pip install` command fails with a broken environment, and I'm left wondering why my new `uv venv` was not used.

It'd be nice to see

```
hint: `VIRTUAL_ENV` is set to another path (...), consider clearing it with `deactivate`
```

We probably don't want to show the same hint when a non-default name is used, because uv won't discover those by default.

---

_Label `error messages` added by @zanieb on 2025-07-21 15:53_

---

_Label `cli` added by @zanieb on 2025-07-21 15:53_

---
