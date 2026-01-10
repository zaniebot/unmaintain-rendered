```yaml
number: 11408
title: PLR0912 seems to overcount branches
type: issue
state: closed
author: jaap3
labels:
  - documentation
  - good first issue
  - question
assignees: []
created_at: 2024-05-13T15:36:42Z
updated_at: 2024-05-16T02:17:06Z
url: https://github.com/astral-sh/ruff/issues/11408
synced_at: 2026-01-10T11:09:53Z
```

# PLR0912 seems to overcount branches

---

_Issue opened by @jaap3 on 2024-05-13 15:36_

ruff 0.4.4 triggers `PLR0912 Too many branches (13 > 12)` on this code snippet:

```python
def foo(bar_list, baz_list):
    if len(bar_list) != len(baz_list):
        raise ValueError("Both lists must have the same length")

    for bar in bar_list:
        if bar.foobar:
            try:
                handle_foobar_bar(bar)
            except SomeError:
                logger.exception("Error handling bar")
            else:
                success(bar)

    for baz in baz_list:
        try:
            f = baz.file.open("rb")
        except FileNotFoundError:
            logger.exception("File not found")
            continue

        with f:
            try:
                handle_baz_file(f)
            except SomeError as e:
                logger.exception("Error handling baz")
                if isinstance(e, SpecificError):
                    handle_specific_error(baz, e)
            else:
                success(baz)
```

I can only get to 13 branches by counting each for loop as a branch and including try/except as branches (related #11205). 

Is this intended?

---

_Comment by @zanieb on 2024-05-13 15:53_

I believe this matches the upstream implementation e.g. 

> Not only if clauses are branches, also loops and try-except clauses have conditional characteristics and thus are counted as well.

https://pycodequ.al/docs/pylint-messages/r0912-too-many-branches.html

---

_Label `question` added by @zanieb on 2024-05-13 15:53_

---

_Comment by @jaap3 on 2024-05-13 18:33_

@zanieb ah, thanks for pointing that out. I only looked at https://docs.astral.sh/ruff/rules/too-many-branches/ and https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/too-many-branches.html and didn't find any mention of loops or try/except there.

---

_Comment by @zanieb on 2024-05-13 21:03_

I'd welcome a PR improving that documentation. Idk why it's omitted.

---

_Label `documentation` added by @zanieb on 2024-05-13 21:03_

---

_Label `good first issue` added by @zanieb on 2024-05-13 21:03_

---

_Closed by @charliermarsh on 2024-05-16 02:17_

---
