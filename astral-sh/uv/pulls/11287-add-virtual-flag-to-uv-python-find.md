```yaml
number: 11287
title: "Add `--virtual` flag to `uv python find`"
type: pull_request
state: open
author: zanieb
labels:
  - cli
assignees: []
draft: true
base: main
head: zb/find-virtual
created_at: 2025-02-06T18:31:07Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11287
synced_at: 2026-01-10T11:10:35Z
```

# Add `--virtual` flag to `uv python find`

---

_Pull request opened by @zanieb on 2025-02-06 18:31_

Mainly useful for testing, confusing that this was not available

---

_Label `cli` added by @zanieb on 2025-02-06 18:31_

---

_Converted to draft by @zanieb on 2025-02-06 18:45_

---

_Comment by @zanieb on 2025-02-06 18:46_

Going to leave this as a draft for a minute. Posted so it didn't get lost. I thought I needed this for test coverage in https://github.com/astral-sh/uv/pull/11218 but it turns out we can write tests there based on the interpreter path and don't need a "failure" to indicate what interpreter we consider virtual.

---
