---
number: 9817
title: "BUG: ruff 0.1.9 and 0.2.0 don't seem to respect noqa comments for NPY rules"
type: issue
state: closed
author: neutrinoceros
labels:
  - question
assignees: []
created_at: 2024-02-04T21:07:57Z
updated_at: 2024-02-05T06:25:21Z
url: https://github.com/astral-sh/ruff/issues/9817
synced_at: 2026-01-07T13:12:15-06:00
---

# BUG: ruff 0.1.9 and 0.2.0 don't seem to respect noqa comments for NPY rules

---

_Issue opened by @neutrinoceros on 2024-02-04 21:07_

Following https://github.com/numpy/numpy/pull/25738, I tried updating my code to only use non-deprecated numpy API as follow

```python
if NUMPY_VERSION >= (2, 0, 0):
    from numpy import trapezoid as trapezoid
else:
    from numpy import trapz as trapezoid  # noqa: NPY003
```

ruff keeps reformatting to
```python
if NUMPY_VERSION >= (2, 0, 0):
    from numpy import trapezoid as trapezoid
else:
    pass  # noqa: NPY003
```
Not only does seem to ignore my noqa comments, it also feels broken that it would just remove the import statement altogether. This happens in a module that's just there to provide a compatibility layer, and doesn't use the `trapezoid` function, so I'm guessing it's playing badly with [F401](https://docs.astral.sh/ruff/rules/unused-import/) ?

I've tried this with ruff version 0.1.9 and 0.2.0, to no avail.
Also tried with `# noqa: NPY201` and `# noqa: NPY`.



---

_Comment by @charliermarsh on 2024-02-05 00:36_

@neutrinoceros - I think Ruff just has no way to know that the import exists for the purpose of a re-export. (We avoid treating `from foo import x as x` imports as unused, since that's a de facto pattern for re-exporting members.) Do you have a suggestion for how Ruff would "know" not to remove the import here?


---

_Label `question` added by @charliermarsh on 2024-02-05 00:36_

---

_Comment by @charliermarsh on 2024-02-05 00:36_

Separately, it should be `# noqa: F401`, not `# noqa: NPY003` -- since the rule that's causing the import to be removed is `F401`, so that's the rule you want to suppress.

---

_Comment by @neutrinoceros on 2024-02-05 06:25_

> it should be # noqa: F401, not # noqa: NPY003

damnit, I was so close to the right idea. What got me is that I didn't actually research the lint rule number for unused import *before* I wrote the ticket 😅 

> Do you have a suggestion for how Ruff would "know" not to remove the import here?

I don't, `# noqa: F401` seems sufficient actually ! thank you very much for your input. I think we can close this now !

---

_Closed by @neutrinoceros on 2024-02-05 06:25_

---
