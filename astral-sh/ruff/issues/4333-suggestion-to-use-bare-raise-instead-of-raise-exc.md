```yaml
number: 4333
title: "Suggestion to use bare `raise` instead of `raise exc`"
type: issue
state: closed
author: zanieb
labels:
  - question
assignees: []
created_at: 2023-05-09T22:44:59Z
updated_at: 2023-07-22T00:41:39Z
url: https://github.com/astral-sh/ruff/issues/4333
synced_at: 2026-01-12T15:54:44Z
```

# Suggestion to use bare `raise` instead of `raise exc`

---

_@zanieb_

I've been championing the use of `raise` instead of `raise exc` when re-raising a captured exception as it removes a frame from the traceback. When many exceptions are chained, this can make a significant difference in the length of a traceback.


WRONG: Extra frame included in traceback
```
try:
    raise ValueError("1")
except Exception as exc:
    raise exc
```
```
Traceback (most recent call last):
  File "/Users/mz/dev/prefect/example.py", line 4, in <module>
    raise exc
  File "/Users/mz/dev/prefect/example.py", line 2, in <module>
    raise ValueError("1")
ValueError: 1
```

RIGHT: No frame added when the exception is re-raised
```python
try:
    raise ValueError("1")
except Exception:
    raise
```
```
Traceback (most recent call last):
  File "/Users/mz/dev/prefect/example.py", line 2, in <module>
    raise ValueError("1")
ValueError: 1
```

I don't think this is covered by any existing linters. I checked `flake8` and `pylint`.

---

_Comment by @tjkuson on 2023-05-09 23:08_

As I understand it, this is covered by [tryceratops](https://github.com/guilatrova/tryceratops) and is implemented in ruff as [`TRY201`](https://beta.ruff.rs/docs/rules/verbose-raise/).

---

_Label `question` added by @charliermarsh on 2023-05-10 02:53_

---

_Comment by @charliermarsh on 2023-05-10 14:28_

Thanks for the clear issue :) My read is that this is covered by `TRY201` as @tjkuson mentioned -- but just LMK if I'm misunderstanding.

---

_Closed by @charliermarsh on 2023-05-10 14:28_

---

_Comment by @zanieb on 2023-05-10 15:15_

Cool! I'm interested in adding explanation there as they call it "redundant" but it has an actual runtime effect.

What kind of criteria is there for a rule being included by default?

I've noticed this is also not an auto-fixable issue but it seems like it could be

```
example.py:4:11: TRY201 Use `raise` without specifying exception name
Found 1 error.
```

---

_Comment by @tylerlaprade on 2023-07-22 00:41_

Hi @zanieb and @charliermarsh, I would also enjoy seeing this rule become auto-fixable.

---
