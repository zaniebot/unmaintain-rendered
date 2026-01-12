```yaml
number: 22118
title: Fix iter example in usafe fixes doc
type: pull_request
state: merged
author: PeterJCLaw
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-fix-safety-docs-example
created_at: 2025-12-20T21:03:34Z
updated_at: 2025-12-22T14:25:28Z
url: https://github.com/astral-sh/ruff/pull/22118
synced_at: 2026-01-12T15:57:42Z
```

# Fix iter example in usafe fixes doc

---

_@PeterJCLaw_

## Summary

This appears to have been a copy/paste error from the list example, as the subscript is not present in the original next/iter example only in the case where the error case is shown. While in the specific example code the subscript actually has no effect, it does make the example slightly confusing.

Consider the following variations, first the example from the docs unchanged and second the same code but not hitting the intended error case (due to using a non-empty collection):
```console
$ python3 -c 'next(iter(range(0)))[0]'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
StopIteration

$ python3 -c 'next(iter(range(1)))[0]'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
TypeError: 'int' object is not subscriptable
```

## Test Plan

Not directly tested, however see inline snippets above.

---

_Marked ready for review by @PeterJCLaw on 2025-12-20 21:15_

---

_@ntBre approved on 2025-12-22 14:23_

Thank you!

---

_Label `documentation` added by @ntBre on 2025-12-22 14:24_

---

_Merged by @ntBre on 2025-12-22 14:25_

---

_Closed by @ntBre on 2025-12-22 14:25_

---
