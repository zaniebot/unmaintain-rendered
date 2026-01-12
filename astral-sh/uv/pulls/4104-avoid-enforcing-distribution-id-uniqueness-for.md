```yaml
number: 4104
title: Avoid enforcing distribution ID uniqueness for extras
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/l
created_at: 2024-06-06T15:15:23Z
updated_at: 2024-06-06T15:49:47Z
url: https://github.com/astral-sh/uv/pull/4104
synced_at: 2026-01-12T16:06:02Z
```

# Avoid enforcing distribution ID uniqueness for extras

---

_@charliermarsh_

## Summary

The condition enforced here isn't quite right. The same dependency can appear multiple times, as long as the extra is different.

Closes https://github.com/astral-sh/uv/issues/4101.

---

_Label `bug` added by @charliermarsh on 2024-06-06 15:15_

---

_Comment by @charliermarsh on 2024-06-06 15:15_

I want to do some more refactors to make this mistake harder / clearer in the future, but I'll do that separately.

---

_Label `preview` added by @charliermarsh on 2024-06-06 15:15_

---

_Merged by @charliermarsh on 2024-06-06 15:21_

---

_Closed by @charliermarsh on 2024-06-06 15:21_

---

_Branch deleted on 2024-06-06 15:21_

---

_@BurntSushi approved on 2024-06-06 15:23_

Yeah I think the original intent here is that an ID would uniquely identify a distribution, and that a `Dependency` was just a reference to a distribution. But a `Dependency` has evolved a bit to include additional information (and it might include more), so I think this is right...

---

_Comment by @charliermarsh on 2024-06-06 15:49_

Yeah I might change this to `DependencyId` or something?

---
