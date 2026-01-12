```yaml
number: 2106
title: "Fix outdated description of ruff's support of isort settings"
type: pull_request
state: merged
author: tmke8
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-01-23T12:28:15Z
updated_at: 2023-01-23T17:29:49Z
url: https://github.com/astral-sh/ruff/pull/2106
synced_at: 2026-01-12T15:55:07Z
```

# Fix outdated description of ruff's support of isort settings

---

_@tmke8_

Ruff supports more than `known-first-party`, `known-third-party`, `extra-standard-library`, and `src` nowadays.

Not sure if this is the best wording. Suggestions welcome!

---

_@JonathanPlasse reviewed on 2023-01-23 17:11_

---

_Review comment by @JonathanPlasse on `README.md`:1646 on 2023-01-23 17:11_

```suggestion
in [Options](#isort). For example, you can set `known-first-party` like so:
```
Maybe link to the `isort` part of the reference.

---

_Merged by @charliermarsh on 2023-01-23 17:29_

---

_Closed by @charliermarsh on 2023-01-23 17:29_

---

_Comment by @charliermarsh on 2023-01-23 17:29_

Thanks!

---
