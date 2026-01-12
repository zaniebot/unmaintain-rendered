```yaml
number: 4157
title: Lockfile omits base package when extra has a marker
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-08T01:13:26Z
updated_at: 2024-06-10T12:40:52Z
url: https://github.com/astral-sh/uv/issues/4157
synced_at: 2026-01-12T15:58:48Z
```

# Lockfile omits base package when extra has a marker

---

_@charliermarsh_

See: https://github.com/astral-sh/uv/pull/4156

---

_Label `bug` added by @charliermarsh on 2024-06-08 01:13_

---

_Label `preview` added by @charliermarsh on 2024-06-08 01:13_

---

_Comment by @charliermarsh on 2024-06-08 01:13_

This is the same root cause as https://github.com/astral-sh/uv/issues/4125 but _slightly_ different due to the use of an extra so worth tracking separately.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-08 01:13_

---

_Comment by @charliermarsh on 2024-06-08 02:13_

I think this one is actually not possible to fix without putting the markers on `Dependency` _or_ returning to the previous strategy of representing each (package, extra) pair as its own `Distribution` in the lockfile.

Because right now, there's no way to represent "a package should be included, but its extra should only be included conditionally" -- it appears impossible given the schema, though I could be mistaken.

\cc @BurntSushi 


---

_Closed by @charliermarsh on 2024-06-10 12:40_

---
