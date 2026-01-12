```yaml
number: 1415
title: "PyUpgrade: Replace pipes with `capture_output=True`"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: remove_pipe
created_at: 2022-12-28T01:04:55Z
updated_at: 2022-12-28T21:53:35Z
url: https://github.com/astral-sh/ruff/pull/1415
synced_at: 2026-01-12T05:36:31Z
```

# PyUpgrade: Replace pipes with `capture_output=True`

---

_Pull request opened by @colin99d on 2022-12-28 01:04_

Creating this PR for visibility. It will remain a draft until the following two issues are resolved:

- I am not sure how to avoid double counting errors, since there are two different ranges and two different fixes need to be applied.
- Currently the `autofix::Fix::deletion` seems to be leaving extra dangling commas and spaces. I am assuming this is my error, and will dig in for a fix.

Update: Both of these fixed, lets get this merged.

A part of #827 

---

_@charliermarsh reviewed on 2022-12-28 02:11_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/UP022.py`:22 on 2022-12-28 02:11_

Yeah this is a bit tricky, since the kwargs can be non-consecutive, and we need to fix it with a single edit operation (one `::replacement` call).

I think the way to approach it would be:

- Find the first kwarg.
- Find the second kwarg.
- Find all content in-between (use `SourceCodeLocator` to slice the range between the two kwargs).
- Treat the fix as: replace (start of first kwarg through to end of second kwarg) with (`capture_output=True` + content between the two kwargs).

---

_@colin99d reviewed on 2022-12-28 02:19_

---

_Review comment by @colin99d on `resources/test/fixtures/pyupgrade/UP022.py`:22 on 2022-12-28 02:19_

Perfect, I like this solution.

---

_@charliermarsh reviewed on 2022-12-28 02:21_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/UP022.py`:22 on 2022-12-28 02:21_

It'd be nice if we could support "multi-fix edits", but it's kind of tricky.

---

_Marked ready for review by @colin99d on 2022-12-28 12:58_

---

_Review comment by @colin99d on `resources/test/fixtures/pyupgrade/UP022.py`:22 on 2022-12-28 12:59_

I figured it out using your solution, but we could just make a bunch of fix functions. Like replace_2_lines, or replace_1_delete_1. But for this specific case. I think what we have now is best.

---

_@colin99d reviewed on 2022-12-28 12:59_

---

_Comment by @charliermarsh on 2022-12-28 16:04_

Sweet, will review today.

---

_Comment by @squiddy on 2022-12-28 16:15_

May I suggest to include test cases from pyupgrade? https://github.com/asottile/pyupgrade/blob/main/tests/features/capture_output_test.py

Examples I don't see covered:

```python
run(["foo"], stdout=None, stderr=PIPE)

from foo import PIPE
from subprocess import run
subprocess.run(["foo"], stdout=PIPE, stderr=PIPE)
```

---

_Comment by @charliermarsh on 2022-12-28 16:23_

(Oh, yes please, I forgot that we were including the pyupgrade test cases.)

---

_Comment by @colin99d on 2022-12-28 16:24_

I will add them for both of my PRs

---

_Comment by @charliermarsh on 2022-12-28 16:25_

🙏 Thank you

---

_Comment by @colin99d on 2022-12-28 16:31_

She passed em flawlessly!

---

_Merged by @charliermarsh on 2022-12-28 21:53_

---

_Closed by @charliermarsh on 2022-12-28 21:53_

---
