```yaml
number: 2903
title: "Merge convert-loop-to-any & convert-loop-to-all to reimplemented-builtin"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: reimplemented-builtin
created_at: 2023-02-14T22:20:24Z
updated_at: 2023-02-16T17:21:18Z
url: https://github.com/astral-sh/ruff/pull/2903
synced_at: 2026-01-12T15:55:12Z
```

# Merge convert-loop-to-any & convert-loop-to-all to reimplemented-builtin

---

_@not-my-profile_

Part of #2714.

---

_Comment by @charliermarsh on 2023-02-14 23:25_

I'm cool with merging these, but should we consider a more refined name? Like `reimplemented-loop-builtin`? `reimplemented-builtin` feels a bit too broad vis-a-vis the actual scope of the rule.

---

_Comment by @not-my-profile on 2023-02-15 00:01_

There aren't that many builtin functions. If you look at the [list of builtin functions](https://docs.python.org/3/library/functions.html#built-in-funcs), I think it would be quite feasible that the lint would also check for abs, enumerate, filter, len, map, max, min, pow, round, sum and zip.

---

_Comment by @charliermarsh on 2023-02-15 04:15_

Mmm ok, that's fair!

I may merge this after the next release (tomorrow, probably), but I can own rebasing etc. as needed.

---

_Merged by @charliermarsh on 2023-02-15 21:24_

---

_Closed by @charliermarsh on 2023-02-15 21:24_

---

_Comment by @not-my-profile on 2023-02-15 21:40_

Opened #2942 to track the implementation of further reimplemented builtin checks.

---

_Comment by @charliermarsh on 2023-02-16 16:52_

@not-my-profile - This is technically a breaking change, right? Since if a user had `# noqa: SIM111`, the suppressed error would now show up as `SIM110`. Or am I misunderstanding?

---

_Comment by @not-my-profile on 2023-02-16 17:14_

I redirected SIM111 to SIM110, so I think ignoring either of these codes should work ... at least that's what I intended.

---

_Comment by @charliermarsh on 2023-02-16 17:21_

Oh sorry, yes, I think you're probably right.

---
