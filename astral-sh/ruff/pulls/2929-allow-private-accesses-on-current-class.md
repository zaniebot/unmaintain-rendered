```yaml
number: 2929
title: Allow private accesses on current class
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/slf
created_at: 2023-02-15T16:47:02Z
updated_at: 2023-02-15T21:49:00Z
url: https://github.com/astral-sh/ruff/pull/2929
synced_at: 2026-01-12T15:55:12Z
```

# Allow private accesses on current class

---

_@charliermarsh_

Closes #2893.

---

_Merged by @charliermarsh on 2023-02-15 16:52_

---

_Closed by @charliermarsh on 2023-02-15 16:52_

---

_Branch deleted on 2023-02-15 16:52_

---

_Comment by @sanmai-NL on 2023-02-15 21:45_

@charliermarsh Does this support the [`Self`](https://docs.python.org/3/whatsnew/3.11.html#whatsnew311-pep673) alias?

---

_Comment by @charliermarsh on 2023-02-15 21:49_

@sanmai-NL - Can you give an example of what you mean by that? `Self` is an annotation. Are you asking if this will allow accesses on variables annotated with `Self`? If so, the answer is no, since we don't leverage type annotations right now.

---
