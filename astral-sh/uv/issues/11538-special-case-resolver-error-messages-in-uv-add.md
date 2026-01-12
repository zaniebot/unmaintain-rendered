```yaml
number: 11538
title: "Special-case resolver error messages in `uv add`"
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-02-15T16:14:44Z
updated_at: 2025-02-15T16:14:47Z
url: https://github.com/astral-sh/uv/issues/11538
synced_at: 2026-01-12T16:00:39Z
```

# Special-case resolver error messages in `uv add`

---

_@zanieb_

This example from https://www.bitecode.dev/p/a-year-of-uv-pros-cons-and-should is chosen to demonstrate our resolver error messages, but honestly I think it's not great :)

```
❯ uv add httpie==2
  × No solution found when resolving dependencies for split (python_full_version >= '3.10'):
  ╰─▶ Because httpie==2.0.0 depends on requests>=2.22.0 and your project depends on httpie==2, we can conclude that your project depends on requests>=2.22.0.
      And because your project depends on requests==1, we can conclude that your project's requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

> Because httpie==2.0.0 depends on requests>=2.22.0 and your project depends on httpie==2, we can conclude that your project depends on requests>=2.22.0.

Our project doesn't depend on `httpie==2` yet, we're trying to add it. Under the hood, we add it as a dependency and solve — but we could special case the added packages .

It'd also be nice to avoid repeating a clause with mixed representations of the version "httpie==2.0.0" and "httpie==2". This might be hard though.

---

_Label `error messages` added by @zanieb on 2025-02-15 16:14_

---
