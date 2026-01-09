---
number: 8695
title: D208 should outdent whole blocks and not line-byline
type: issue
state: closed
author: ThiefMaster
labels:
  - docstring
assignees: []
created_at: 2023-11-15T15:25:02Z
updated_at: 2023-11-17T04:11:09Z
url: https://github.com/astral-sh/ruff/issues/8695
synced_at: 2026-01-07T13:12:15-06:00
---

# D208 should outdent whole blocks and not line-byline

---

_Issue opened by @ThiefMaster on 2023-11-15 15:25_

```python
def test(categ_ids, start_dt, end_dt):
    """Retrieve time blocks that fall within a specific time interval.

       :param categ_ids: iterable containing list of category IDs
       :param start_dt: start of search interval (``datetime``, expected
                        to be in display timezone)
       :param end_dt: end of search interval (``datetime`` in expected
                      to be in display timezone)
    """
```

`ruff --isolated --select D208 --fix` does this:

```python
def test(categ_ids, start_dt, end_dt):
    """Retrieve time blocks that fall within a specific time interval.

    :param categ_ids: iterable containing list of category IDs
    :param start_dt: start of search interval (``datetime``, expected
    to be in display timezone)
    :param end_dt: end of search interval (``datetime`` in expected
    to be in display timezone)
    """
```

But this would be sufficient to fix the violation and preserve the intent of the indentation that's not part of the over-indentation:

```python
def test(categ_ids, start_dt, end_dt):
    """Retrieve time blocks that fall within a specific time interval.

    :param categ_ids: iterable containing list of category IDs
    :param start_dt: start of search interval (``datetime``, expected
                     to be in display timezone)
    :param end_dt: end of search interval (``datetime`` in expected
                   to be in display timezone)
    """
```

IMHO the behavior of the fix should be less aggressive or the fix should be marked as unsafe by default.

`D207` should probably be adapted likewise when indenting under-indented docstrings.

---

_Referenced in [astral-sh/ruff#8699](../../astral-sh/ruff/pulls/8699.md) on 2023-11-15 17:30_

---

_Label `docstring` added by @charliermarsh on 2023-11-15 17:37_

---

_Comment by @zanieb on 2023-11-15 17:39_

Thanks for the report, let me know if https://github.com/astral-sh/ruff/pull/8699 resolves your problems.

---

_Comment by @ThiefMaster on 2023-11-15 18:57_

Yes, works exactly as I was hoping for - thanks for the quick fix!

---

_Closed by @zanieb on 2023-11-17 04:11_

---
