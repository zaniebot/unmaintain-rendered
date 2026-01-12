```yaml
number: 1454
title: "Add custom messages to the `Fix` API"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-12-29T23:52:12Z
updated_at: 2022-12-31T04:16:05Z
url: https://github.com/astral-sh/ruff/issues/1454
synced_at: 2026-01-12T15:54:41Z
```

# Add custom messages to the `Fix` API

---

_@charliermarsh_

In LSP extensions, the action prompt is, like, `Fix F401`. We can make these a lot more specific and actionable (e.g., `Remove import Foo`).


---

_Label `enhancement` added by @charliermarsh on 2022-12-29 23:52_

---

_Comment by @squiddy on 2022-12-30 07:48_

Looks related to #218.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-30 16:21_

---

_Comment by @charliermarsh on 2022-12-30 16:25_

Gonna start on this. I'll make it optional, and add a few common cases.

---

_Closed by @charliermarsh on 2022-12-31 04:16_

---
