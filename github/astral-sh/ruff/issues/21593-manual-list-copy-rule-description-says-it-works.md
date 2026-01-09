---
number: 21593
title: "`manual-list-copy` rule description says it works on existing lists, but it triggers on any iterable"
type: issue
state: open
author: injust
labels:
  - documentation
assignees: []
created_at: 2025-11-23T12:04:07Z
updated_at: 2025-11-25T18:27:14Z
url: https://github.com/astral-sh/ruff/issues/21593
synced_at: 2026-01-07T13:12:16-06:00
---

# `manual-list-copy` rule description says it works on existing lists, but it triggers on any iterable

---

_Issue opened by @injust on 2025-11-23 12:04_

### Summary

On https://docs.astral.sh/ruff/rules/manual-list-copy/, the description says:

> When creating a copy of an existing list using a for-loop, prefer `list` or `list.copy` instead.

And the example suggests:

```python
original = list(range(10000))
filtered = list(original)
```

In Zed, Ruff surfaces this messaging:

> Use `list` or `list.copy` to create a copy of a list

---

`lst.copy()` should be preferred over `list(lst)` so the intention is more explicit (copying vs converting a non-list iterable to a list).

I think the example should be updated to use `list.copy()`, and either:

- Order `list.copy()` before `list()`, or
- Only suggest `list.copy()`

---

_Comment by @amyreese on 2025-11-24 22:54_

My main concern with `data.copy()` this is that you need to know that `data` is an actual list type that implements the copy method, while `list(data)` works regardless of the type of `data` — the latter feels more "pythonic", and its performance is roughly the same[1], so IMO should be the preferred suggestion.

1:
```
amethyst@lunatone ~/scratch » uvx python -m timeit -s 'data = list(range(10000))' 'f = list(data)'
20000 loops, best of 5: 12.8 usec per loop

amethyst@lunatone ~/scratch » uvx python -m timeit -s 'data = list(range(10000))' 'f = data.copy()'
20000 loops, best of 5: 12.2 usec per loop
```

---

_Label `question` added by @amyreese on 2025-11-24 22:54_

---

_Comment by @injust on 2025-11-25 08:27_

The rule's description really needs to be updated. It says it's for making copies of existing lists, but it triggers for any iterable.

> Checks for `for` loops that can be replaced by a making a copy of a list.

This should be something like:

> Checks for `for` loops that can be replaced with `list()`.

And "Why is this bad?" needs to be updated to remove mentions of an "existing list".

---

_Renamed from "`manual-list-copy` rule should prefer `lst.copy()` over `list(lst)` to be more explicit" to "`manual-list-copy` rule description says it works on existing lists, but it triggers on any iterable" by @injust on 2025-11-25 08:27_

---

_Label `question` removed by @MichaReiser on 2025-11-25 18:27_

---

_Label `documentation` added by @MichaReiser on 2025-11-25 18:27_

---
