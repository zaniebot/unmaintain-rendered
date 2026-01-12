```yaml
number: 731
title: "Make `combine-as-imports` the default import sorting behavior"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/combine-as-imports
created_at: 2022-11-14T03:59:26Z
updated_at: 2022-11-14T04:07:13Z
url: https://github.com/astral-sh/ruff/pull/731
synced_at: 2026-01-12T05:48:45Z
```

# Make `combine-as-imports` the default import sorting behavior

---

_Pull request opened by @charliermarsh on 2022-11-14 03:59_

It's hard to be 1:1 identical with isort, but this gets much closer.

The main differences left are...

(1) In some cases, `isort` seems to break ties by deferring to the order that imports were introduced, rather than deterministically based on the content. For example, isort will leave this as-is:

```py
import A
import a
import b
import B
```

I'd rather that Ruff sorts deterministically based on content, and not based on order, so instead Ruff orders those lexicographically.

(2) `isort` does some complicated stuff whereby it doesn't group `from` imports that are broken up lexicographically by a re-export.

For example, `isort` will do this:

```py
# Before
from a import b
from a import d

# After
from a import b, d
```

But also this (leaves unchanged):

```py
# Before
from a import b
from a import c as c
from a import d

# After
from a import b
from a import c as c
from a import d
```

Resolves #705.

---

_Merged by @charliermarsh on 2022-11-14 04:07_

---

_Closed by @charliermarsh on 2022-11-14 04:07_

---

_Branch deleted on 2022-11-14 04:07_

---
