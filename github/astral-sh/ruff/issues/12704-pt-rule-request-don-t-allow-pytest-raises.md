---
number: 12704
title: "PT Rule Request: Don't allow `pytest.raises(AssertionError)` with `assert` in body"
type: issue
state: open
author: notatallshaw
labels:
  - rule
assignees: []
created_at: 2024-08-05T23:38:37Z
updated_at: 2024-08-06T06:21:15Z
url: https://github.com/astral-sh/ruff/issues/12704
synced_at: 2026-01-07T13:12:15-06:00
---

# PT Rule Request: Don't allow `pytest.raises(AssertionError)` with `assert` in body

---

_Issue opened by @notatallshaw on 2024-08-05 23:38_

I am working on adding [pytest rules for pip](https://github.com/pypa/pip/pull/12894). And it highlighted [a test](https://github.com/pypa/pip/blob/3259c061fb36698339877bbf2cefb5e82815e812/tests/functional/test_install_reqs.py#L627) with an interesting anti-pattern:

```python
with pytest.raises(AssertionError):
    assert something in foo
```

Instead of:

```python
assert something not in foo
```

The `pytest.raises(AssertionError)` is redundant here, and it creates a confusing test flow to follow, the test condition should simply be reversed.

I assume a similiar issue could happen with a `try/except AssertionError`

---

_Renamed from "PT Rule Request: Don't allow `pytest.raises(AssertionError)`" to "PT Rule Request: Don't allow `pytest.raises(AssertionError)` with `assert` in body" by @notatallshaw on 2024-08-05 23:48_

---

_Referenced in [pypa/pip#12894](../../pypa/pip/pulls/12894.md) on 2024-08-05 23:58_

---

_Label `rule` added by @charliermarsh on 2024-08-06 02:30_

---

_Comment by @charliermarsh on 2024-08-06 02:31_

This seems reasonable to me.

---

_Comment by @charliermarsh on 2024-08-06 02:31_

@RonnyPfannschmidt may have an opinion here.

---

_Comment by @RonnyPfannschmidt on 2024-08-06 06:04_

Raises with just assertion error should only be allowed if no assert is in the body 

Its reasonable to also require either a match parameter or follow-up assertions on the Exception info returned 

The transformation of the previously mentioned with block to an negated assertion ought to be a unsafe fix 

---

_Comment by @notatallshaw on 2024-08-06 06:08_

Oh yeah, the example was for demonstration purposes, not as an example to auto fix .

---

_Comment by @RonnyPfannschmidt on 2024-08-06 06:21_

It's still useful if the language server can propose a fix 

---
