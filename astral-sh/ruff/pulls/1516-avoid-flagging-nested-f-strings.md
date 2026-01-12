```yaml
number: 1516
title: Avoid flagging nested f-strings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ignore
created_at: 2022-12-31T18:39:30Z
updated_at: 2022-12-31T18:41:22Z
url: https://github.com/astral-sh/ruff/pull/1516
synced_at: 2026-01-12T05:36:31Z
```

# Avoid flagging nested f-strings

---

_Pull request opened by @charliermarsh on 2022-12-31 18:39_

We're currently flagging cases like this:

```py
v = 23.234234
f"{v:0.2f}"  # error
```

Because the `0.2f` is represented as an "empty" `JoinedStr`. However, this is consistent with CPython. Pyflakes just skips these, which leads to some false negatives... but it definitely outweighs the false positives, which would be common.

Resolves #1514.

---

_Merged by @charliermarsh on 2022-12-31 18:41_

---

_Closed by @charliermarsh on 2022-12-31 18:41_

---

_Branch deleted on 2022-12-31 18:41_

---
