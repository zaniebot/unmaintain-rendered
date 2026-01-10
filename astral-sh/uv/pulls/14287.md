```yaml
number: 14287
title: "Use `flit_editable` instead of `poetry_editable` throughout"
type: pull_request
state: open
author: zanieb
labels:
  - testing
assignees: []
draft: true
base: main
head: zb/flit-v-poetry
created_at: 2025-06-26T17:38:59Z
updated_at: 2025-06-27T22:05:29Z
url: https://github.com/astral-sh/uv/pull/14287
synced_at: 2026-01-10T06:53:01Z
```

# Use `flit_editable` instead of `poetry_editable` throughout

---

_Pull request opened by @zanieb on 2025-06-26 17:38_

Extends https://github.com/astral-sh/uv/pull/14285 to more of the test suite, retaining a couple tests covering the `poetry_editable` case.

---

_Label `testing` added by @zanieb on 2025-06-26 17:39_

---

_Comment by @zanieb on 2025-06-27 22:05_

Some of these require a dependency, like `list_editable_only` -.-

Will need to do a little more work here.

---
