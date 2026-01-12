```yaml
number: 1544
title: "Implement `yield`-to-`yield from` conversion"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: yieldfrom
created_at: 2023-01-02T03:26:17Z
updated_at: 2023-01-05T15:37:44Z
url: https://github.com/astral-sh/ruff/pull/1544
synced_at: 2026-01-12T15:55:06Z
```

# Implement `yield`-to-`yield from` conversion

---

_@colin99d_

A part of #827.

I am creating this draft pr for visibility. This is easily the most challenging fixer I have done, due to the infinite amount of nesting of functions inside functions allowed in python. Because of this I have created a recursive function to get all potential yield from statements. There is still a lot of work to be done, but I just wanted to push this in case anyone sees a much simpler way to do it.

---

_Marked ready for review by @colin99d on 2023-01-02 17:47_

---

_Comment by @colin99d on 2023-01-02 17:50_

This is done and ready for review. It passes all the tests provided by pyupgrade except for one case I document in the code. Current issues are as follows:
1. Our implementation keeps more comments than pyupgrade does (this is the issue mentioned above). However, I think this is more of a plus than a minus. I REALLY doubt people want their comments removed by our linter.
2. The implementation would fail if before or after the loop x is defined as a separate thing and used. Our implementation would err in the side of NOT making changes.

---

_Comment by @charliermarsh on 2023-01-03 04:24_

Will try to get to this tomorrow, thanks!

---

_Renamed from "Pyupgrade: Yieldfrom" to "Implement `yield`-to-`yield from` conversion" by @charliermarsh on 2023-01-04 03:56_

---

_Merged by @charliermarsh on 2023-01-04 03:56_

---

_Closed by @charliermarsh on 2023-01-04 03:56_

---

_@charliermarsh reviewed on 2023-01-04 04:03_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_yield_from.rs`:121 on 2023-01-04 04:03_

@colin99d - I ended up refactoring this quite a bit. You might be interested in checking out the current implementation. The logic is roughly: do one pass over the function to find any candidate `yield` statements to convert, do a second pass to find any references to variables, then cross-reference to ensure we don't rewrite any yields whose variables are referenced elsewhere.

---

_Branch deleted on 2023-01-05 15:37_

---
