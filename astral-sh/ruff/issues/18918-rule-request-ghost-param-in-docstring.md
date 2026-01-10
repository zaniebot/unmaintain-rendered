```yaml
number: 18918
title: "rule request: ghost param in docstring"
type: issue
state: closed
author: DaniBodor
labels:
  - question
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-24T15:09:09Z
updated_at: 2025-06-25T08:07:59Z
url: https://github.com/astral-sh/ruff/issues/18918
synced_at: 2026-01-10T11:09:59Z
```

# rule request: ghost param in docstring

---

_Issue opened by @DaniBodor on 2025-06-24 15:09_

### Summary

[rule D417](https://docs.astral.sh/ruff/rules/undocumented-param/) detects undocumented parameters in docstrings. It would be very useful to have the inverse rule as well, which checks if there are parameters listed in the docstring that do not exist (anymore).

It can happen that while refactoring a function an argument gets removed, and it would be useful if ruff automatically detected that the docstring needs to be adjusted accordingly, as these can easily be left behind.

---

_Label `rule` added by @ntBre on 2025-06-24 16:25_

---

_Label `needs-decision` added by @ntBre on 2025-06-24 16:25_

---

_Comment by @ntBre on 2025-06-24 16:29_

It looks like this could be DOC102/DOC103 from [pydoclint](https://jsh9.github.io/pydoclint/violation_codes.html#1-doc1xx-violations-about-input-arguments), in which case this is covered by https://github.com/astral-sh/ruff/issues/12434.

---

_Label `question` added by @MichaReiser on 2025-06-24 20:01_

---

_Comment by @DaniBodor on 2025-06-25 08:07_

> It looks like this could be DOC102/DOC103 from [pydoclint](https://jsh9.github.io/pydoclint/violation_codes.html#1-doc1xx-violations-about-input-arguments), in which case this is covered by [#12434](https://github.com/astral-sh/ruff/issues/12434).

Indeed, I was unaware that this was already in development.

---

_Closed by @DaniBodor on 2025-06-25 08:07_

---
