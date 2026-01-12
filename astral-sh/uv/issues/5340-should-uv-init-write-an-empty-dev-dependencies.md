```yaml
number: 5340
title: "Should `uv init` write an empty `dev-dependencies` section?"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - cli
  - preview
assignees: []
created_at: 2024-07-23T15:55:00Z
updated_at: 2024-07-24T20:33:29Z
url: https://github.com/astral-sh/uv/issues/5340
synced_at: 2026-01-12T15:58:55Z
```

# Should `uv init` write an empty `dev-dependencies` section?

---

_@zanieb_

It seems noisy, the user can add this with `uv add --dev foo`?

```
❯ uv init
warning: `uv init` is experimental and may change without warning
Initialized project example

❯ cat pyproject.toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []

[tool.uv]
dev-dependencies = []
```

---

_Label `cli` added by @zanieb on 2024-07-23 15:55_

---

_Label `preview` added by @zanieb on 2024-07-23 15:55_

---

_Comment by @charliermarsh on 2024-07-23 16:54_

I'm fine either way.

---

_Comment by @zanieb on 2024-07-23 17:09_

It seems weird to write a non-standard entry by default if it's empty. I'd prefer to omit it.

---

_Label `good first issue` added by @zanieb on 2024-07-23 17:09_

---

_Comment by @trag1c on 2024-07-24 20:32_

Looks like the PR didn't get picked up by GitHub's thingy, this should probably be closed :smile:

---

_Comment by @charliermarsh on 2024-07-24 20:33_

Good call, thanks.

---

_Closed by @charliermarsh on 2024-07-24 20:33_

---
