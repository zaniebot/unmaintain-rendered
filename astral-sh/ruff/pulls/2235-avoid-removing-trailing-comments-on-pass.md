```yaml
number: 2235
title: "Avoid removing trailing comments on `pass` statements"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pd
created_at: 2023-01-26T22:09:39Z
updated_at: 2023-01-26T22:29:14Z
url: https://github.com/astral-sh/ruff/pull/2235
synced_at: 2026-01-12T04:52:00Z
```

# Avoid removing trailing comments on `pass` statements

---

_Pull request opened by @charliermarsh on 2023-01-26 22:09_

This isn't super consistent with some other rules, but... if you have a lone body, with a `pass`, followed by a comment, it's probably surprising if it gets removed. Let's retain the comment.

Closes #2231.

---

_Renamed from "Avoid removing pass statements with trailing comments" to "Avoid removing trailing comments on `pass` statements" by @charliermarsh on 2023-01-26 22:27_

---

_Merged by @charliermarsh on 2023-01-26 22:29_

---

_Closed by @charliermarsh on 2023-01-26 22:29_

---

_Branch deleted on 2023-01-26 22:29_

---
