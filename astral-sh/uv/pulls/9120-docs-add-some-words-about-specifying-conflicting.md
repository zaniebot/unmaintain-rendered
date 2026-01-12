```yaml
number: 9120
title: "docs: add some words about specifying conflicting extras/groups"
type: pull_request
state: merged
author: BurntSushi
labels:
  - documentation
assignees: []
merged: true
base: main
head: ag/conflict-docs
created_at: 2024-11-14T15:57:36Z
updated_at: 2024-11-14T19:50:23Z
url: https://github.com/astral-sh/uv/pull/9120
synced_at: 2026-01-12T16:08:40Z
```

# docs: add some words about specifying conflicting extras/groups

---

_@BurntSushi_

This doesn't cover the optional `package` key since I wasn't quite sure
how to articulate its utility in a digestible way.


---

_Review requested from @zanieb by @BurntSushi on 2024-11-14 15:57_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-14 15:57_

---

_Review comment by @konstin on `docs/concepts/projects.md`:530 on 2024-11-14 16:00_

We should add something about workspace here, i expect that most users complex enough to have these problems are also workspace users, often also virtual root users.

---

_@konstin approved on 2024-11-14 16:00_

---

_@BurntSushi reviewed on 2024-11-14 16:11_

---

_Review comment by @BurntSushi on `docs/concepts/projects.md`:530 on 2024-11-14 16:11_

So I don't think there are any extant issues filed about this. All of the examples I came across (like dagster and other issues filed like [this one](https://github.com/astral-sh/uv/issues/8024)) are all just the simple local cases of conflicting extras.

I think I would struggle a bit to describe a use case without it seeming very contrived. (That's why I didn't mention the `package` key.)

---

_Merged by @zanieb on 2024-11-14 19:50_

---

_Closed by @zanieb on 2024-11-14 19:50_

---

_Branch deleted on 2024-11-14 19:50_

---

_Label `documentation` added by @zanieb on 2024-11-14 19:50_

---
