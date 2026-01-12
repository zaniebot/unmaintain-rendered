```yaml
number: 237
title: Correctly display the location of parse errors
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: SyntaxError
created_at: 2022-09-21T00:20:54Z
updated_at: 2022-09-21T02:16:49Z
url: https://github.com/astral-sh/ruff/pull/237
synced_at: 2026-01-12T05:48:45Z
```

# Correctly display the location of parse errors

---

_Pull request opened by @andersk on 2022-09-21 00:20_

Before:

    test.py:0:0: E999 SyntaxError: unexpected EOF while parsing at line 3 column 7

After:

    test.py:3:7: E999 SyntaxError: unexpected EOF while parsing

This also allows line-based checks to run even if there’s a parse error.

---

_Comment by @charliermarsh on 2022-09-21 00:52_

Nice, good call.

---

_@charliermarsh reviewed on 2022-09-21 00:53_

---

_Review comment by @charliermarsh on `src/linter.rs`:38 on 2022-09-21 00:53_

Wonder if we should use a different code here vs. in `main.rs`.

---

_Merged by @charliermarsh on 2022-09-21 00:53_

---

_Closed by @charliermarsh on 2022-09-21 00:53_

---

_Branch deleted on 2022-09-21 00:54_

---

_@andersk reviewed on 2022-09-21 00:55_

---

_Review comment by @andersk on `src/linter.rs`:38 on 2022-09-21 00:55_

Yeah, probably. Flake8 reports `E902` for filesystem errors.

- #240

---
