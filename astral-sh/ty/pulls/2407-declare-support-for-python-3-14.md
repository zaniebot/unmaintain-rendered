```yaml
number: 2407
title: Declare support for Python 3.14
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: dhruv/3-14
created_at: 2026-01-09T04:55:36Z
updated_at: 2026-01-09T20:43:34Z
url: https://github.com/astral-sh/ty/pull/2407
synced_at: 2026-01-12T15:54:28Z
```

# Declare support for Python 3.14

---

_@dhruvmanila_

## Summary

This PR officially declare support for Python 3.14 which I think we do looking at the changelog of 3.14.

Reference: https://docs.python.org/3/whatsnew/3.14.html

- ty always defers annotation on 3.14
- t-strings are supported
- `except` and `except *` are supported without parentheses
- What about https://docs.python.org/3/whatsnew/3.14.html#whatsnew314-finally-syntaxwarning ?


---

_Comment by @MichaReiser on 2026-01-09 07:58_

> What about [docs.python.org/3/whatsnew/3.14.html#whatsnew314-finally-syntaxwarning](https://docs.python.org/3/whatsnew/3.14.html#whatsnew314-finally-syntaxwarning) ?

I don't think this is implemented, see https://play.ruff.rs/fb66ac1a-155e-49f8-9e1b-5dfae415d4da. But I also don't consider syntax warnings as blocking to claim support for a newer Python version.



---

_@MichaReiser approved on 2026-01-09 07:58_

---

_Merged by @dhruvmanila on 2026-01-09 09:01_

---

_Closed by @dhruvmanila on 2026-01-09 09:01_

---

_Branch deleted on 2026-01-09 09:01_

---

_Comment by @carljm on 2026-01-09 20:42_

Also I think adding that syntax warning was a mistake, and I would be fine if ty never emulated it 😆 (EDIT: more seriously, I think it would be a great lint rule for ty in future -- but it shouldn't be a runtime warning.)

---
