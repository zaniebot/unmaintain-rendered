---
number: 1947
title: "Should SIM105 `UseContextlibSuppress` exempt try blocks with unreachable end?"
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2023-01-18T04:48:45Z
updated_at: 2023-01-18T05:22:23Z
url: https://github.com/astral-sh/ruff/issues/1947
synced_at: 2026-01-10T01:22:40Z
---

# Should SIM105 `UseContextlibSuppress` exempt try blocks with unreachable end?

---

_Issue opened by @andersk on 2023-01-18 04:48_

This code violates SIM105 (Use `contextlib.suppress(OSError)` instead of try-except-pass):

```python
for i in range(1, 100):
    try:
        realm.string_id = prefix + str(i)
        realm.save(update_fields=["string_id"])
        break
    except IntegrityError:
        pass
else:
    raise RuntimeError(f"Unable to find a good string_id for realm {realm}")
```

But fixing it would make the code more confusing:

```python
for i in range(1, 100):
    with contextlib.suppress(IntegrityError):
        realm.string_id = prefix + str(i)
        realm.save(update_fields=["string_id"])
        break
else:
    raise RuntimeError(f"Unable to find a good string_id for realm {realm}")
```

because this visually suggests that the `for` loop “always” `break`s and it’s not obvious why the `else` can be reached.

If a `try:` block has an unreachable end because all its non-exceptional control flow paths end with a `return`, `raise`, `break`, or `continue`, should we exempt it from SIM105?

---

_Comment by @charliermarsh on 2023-01-18 04:56_

Yeah I agree that the fixed version is more confusing. (I kind of feel like this rule always makes code more confusing? Though I've been trying to avoid opining too much on rule usefulness while implementing existing, third-party plugins.) I think this suggestion makes sense.


---

_Comment by @andersk on 2023-01-18 05:09_

Upstream doesn’t have this restriction, but it has a different one: only `try` statements with a [single body statement](https://github.com/MartinThoma/flake8-simplify/blob/0.19.3/flake8_simplify/rules/ast_try.py#L46) are considered. I think this also makes sense, because it may not be immediately apparent that a “suppressed” exception in the first body statement still short-circuits the second body statement.

The upstream restriction doesn’t consider whether the single body statement is itself a compound statement, though, and probably should.

---

_Comment by @charliermarsh on 2023-01-18 05:17_

Oh, yeah, that looks like an oversight.

---

_Label `bug` added by @charliermarsh on 2023-01-18 05:17_

---

_Referenced in [astral-sh/ruff#1948](../../astral-sh/ruff/pulls/1948.md) on 2023-01-18 05:19_

---

_Closed by @charliermarsh on 2023-01-18 05:22_

---

_Referenced in [astral-sh/ruff#8593](../../astral-sh/ruff/issues/8593.md) on 2023-11-10 13:18_

---

_Referenced in [GenericMappingTools/pygmt#3448](../../GenericMappingTools/pygmt/pulls/3448.md) on 2024-09-24 01:56_

---

_Referenced in [GenericMappingTools/pygmt#3523](../../GenericMappingTools/pygmt/pulls/3523.md) on 2024-10-16 08:40_

---
