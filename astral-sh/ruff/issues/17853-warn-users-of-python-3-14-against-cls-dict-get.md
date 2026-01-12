```yaml
number: 17853
title: "Warn users of Python 3.14+ against `cls.__dict__.get(\"__annotations__\", {})`"
type: issue
state: closed
author: dylwil3
labels:
  - rule
  - help wanted
  - python314
assignees: []
created_at: 2025-05-05T11:37:22Z
updated_at: 2025-06-20T13:32:41Z
url: https://github.com/astral-sh/ruff/issues/17853
synced_at: 2026-01-12T15:54:56Z
```

# Warn users of Python 3.14+ against `cls.__dict__.get("__annotations__", {})`

---

_@dylwil3_

### Summary

cf https://github.com/astral-sh/ruff/pull/17658#pullrequestreview-2810350730 and https://github.com/astral-sh/ruff/pull/17658#issuecomment-2845922256

---

_Label `rule` added by @dylwil3 on 2025-05-05 11:37_

---

_Renamed from "Warn user's of Python 3.14+ against `cls.__dict__.get("__annotations__", {})`" to "Warn users of Python 3.14+ against `cls.__dict__.get("__annotations__", {})`" by @AlexWaygood on 2025-05-05 11:48_

---

_Comment by @AlexWaygood on 2025-05-05 12:54_

It might actually be good to emit this warning regardless of the target-version that Ruff infers. Just because somebody has `target-version = "py38"` in their config file doesn't mean that they're uninterested in their code working on Python 3.14. And there are more bulletproof ways of accessing the class's annotations that will work on all Python versions (either `inspect.get_annotations` or the `typing_extensions` backport of `annotationlib.get_annotations`).

---

_Label `help wanted` added by @AlexWaygood on 2025-05-19 14:32_

---

_Label `python314` added by @AlexWaygood on 2025-05-19 14:32_

---

_Closed by @ntBre on 2025-06-20 13:32_

---
