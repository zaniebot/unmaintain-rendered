```yaml
number: 2962
title: Avoid trying to fix implicit returns with control flow
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ret
created_at: 2023-02-16T17:44:30Z
updated_at: 2023-02-20T21:45:47Z
url: https://github.com/astral-sh/ruff/pull/2962
synced_at: 2026-01-12T04:39:44Z
```

# Avoid trying to fix implicit returns with control flow

---

_Pull request opened by @charliermarsh on 2023-02-16 17:44_

# Summary

This PR attempts to fix the issues raised in #2948 (by applying fixes to the level above control flow, and adding a newline if absent), though it changes some of the decisions made in the RET503 implementation in the process.

Specifically, we now flag this as an error:

```py
def bar1(x, y, z):
    for i in x:
        return z
```

Whereas before, that was considered acceptable. This makes it easier to enforce and reason about the rule, since we no longer need to look within loops, only beyond them.

(That should arguably have always been an error, since we can't guarantee that the loop even runs, so there _is_ an implicit return.)

Closes #2948.

Closes #2781.

---

_Merged by @charliermarsh on 2023-02-16 18:42_

---

_Closed by @charliermarsh on 2023-02-16 18:42_

---

_Branch deleted on 2023-02-16 18:42_

---

_Comment by @link2xt on 2023-02-20 21:45_

This introduced a regression: #3071 

I had to pin ruff version for now because of this: https://github.com/deltachat/deltachat-core-rust/commit/f07206bd6cb6e4e783a197eeb88b6367f0de9afd

---
