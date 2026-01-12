```yaml
number: 2255
title: Fix SIM300 to take Python constants into account
type: pull_request
state: merged
author: frenck
labels: []
assignees: []
merged: true
base: main
head: frenck-2023-0222
created_at: 2023-01-27T14:40:17Z
updated_at: 2023-01-27T16:25:28Z
url: https://github.com/astral-sh/ruff/pull/2255
synced_at: 2026-01-12T04:52:00Z
```

# Fix SIM300 to take Python constants into account

---

_Pull request opened by @frenck on 2023-01-27 14:40_

SIM300 currently doesn't take Python constants into account when looking for Yoda conditions, this PR fixes that behavior.

```python
# Errors
YODA == age  # SIM300
YODA > age  # SIM300
YODA >= age  # SIM300

# OK
age == YODA
age < YODA
age <= YODA
```

Ref: <https://github.com/home-assistant/core/pull/86793>

---

_Review comment by @epenet on `resources/test/fixtures/flake8_simplify/SIM300.py`:22 on 2023-01-27 15:07_

Maybe add a test for two constants?
`THIS == THAT`

---

_@epenet reviewed on 2023-01-27 15:08_

---

_Review comment by @frenck on `resources/test/fixtures/flake8_simplify/SIM300.py`:22 on 2023-01-27 15:13_

Done in [b348a13](https://github.com/charliermarsh/ruff/pull/2255/commits/b348a133bd80d047aaebfca53ceae709a17b03b7)

---

_@frenck reviewed on 2023-01-27 15:13_

---

_Comment by @charliermarsh on 2023-01-27 15:43_

Just to confirm: I think this is a deviation from `flake8-simplify`'s behavior, so we'd be knowingly extending the rule to some new cases -- is that correct?

---

_Comment by @frenck on 2023-01-27 15:49_

I don't know, I've not used it, but seems like it. Is the goal to be bug-by-bug compatible with upstream?

If that is the case, this rule would already be wrongly implemented, as the upstream comparison only supports equality (`==`): 

<https://github.com/MartinThoma/flake8-simplify/blob/d3cb9556a5255969f83a8329df8f635da41f595d/flake8_simplify/rules/ast_compare.py#L81>

../Frenck

---

_Comment by @charliermarsh on 2023-01-27 15:52_

Nope! We deviate from upstream in some cases. I just try to do so intentionally, so I want to know when we're making an opinionated change.

Let me just run this over a few large codebases and see how it looks.

---

_Merged by @charliermarsh on 2023-01-27 16:20_

---

_Closed by @charliermarsh on 2023-01-27 16:20_

---

_Comment by @charliermarsh on 2023-01-27 16:21_

Ok cool -- this seems reasonable to me! (I pushed a tweak to extract to a separate helper, and use the `Expr` directly rather than the string pulled from the source code -- hope that's fine.)

This'll go out tonight, probably, assuming there's enough stuff to justify a release.

---

_Branch deleted on 2023-01-27 16:24_

---

_Comment by @frenck on 2023-01-27 16:24_

Sure thanks! 👍 

---

_Comment by @charliermarsh on 2023-01-27 16:25_

Thanks for contributing!

---
