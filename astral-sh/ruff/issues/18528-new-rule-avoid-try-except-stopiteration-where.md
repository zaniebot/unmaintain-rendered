```yaml
number: 18528
title: "New rule: Avoid `try-except StopIteration` where `next(..., default)` can be used"
type: issue
state: open
author: spaceone
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-07T06:05:07Z
updated_at: 2025-06-09T16:43:12Z
url: https://github.com/astral-sh/ruff/issues/18528
synced_at: 2026-01-12T15:54:56Z
```

# New rule: Avoid `try-except StopIteration` where `next(..., default)` can be used

---

_@spaceone_

### Summary

A new rule that improves both readability and performance by avoiding `try-except StopIteration` constructs when iterating via `next()` could be added. In many simple statements, using `next(iterator, default)` with an explicit check is clearer and faster than relying on expensive exception handling.

replace:
```python
try:
    value = next(it)
except StopIteration:
    value = None
```

with:
```python
value = next(it, None)
```
where `None` is any sentinel object, which doesn't occur in the iterator.


Example demonstrating performance enhancement:

```python
foo = iter([1, 2, 3, 4, 5, 6, 7, 8, 9])


def v1():
    try:
        return next(foo)  # or whatever logic is done afterwards with it
    except StopIteration:
        return


def v2():
    d = next(foo, None)
    if d is None:
        return
    return d  # or whatever logic is done afterwards with it


def v3():
    if d := next(foo, sentinel := object()) is not sentinel:
        return
    return d  # or whatever logic is done afterwards with it
```

```bash
$ python3 -m timeit -s 'import test_stop' 'while test_stop.v1(): pass'
1000000 loops, best of 5: 232 nsec per loop
$ python3 -m timeit -s 'import test_stop' 'while test_stop.v2(): pass'
5000000 loops, best of 5: 70.2 nsec per loop
$ python3 -m timeit -s 'import test_stop' 'while test_stop.v3(): pass'
2000000 loops, best of 5: 128 nsec per loop
```

When should the rule apply?
* The only statement in the try block is `next(...)`
* No use of `else`, `finally`, or other logic in the exception handler.

It should avoid applying inside generators, which are affected by [PEP 479](https://peps.python.org/pep-0479/).
The suggestion to use `None` as default doesn't work when the iterator yields `None`, so propose some other singleton (e.g. `sentinel = object(); if next(it, sentinel) is not sentinel: `.

Advantages:
* Better performance (no exception handling).
* Cleaner, more idiomatic code.
* Encourages explicit loop termination over implicit exception-based flow control.

---

_Label `rule` added by @ntBre on 2025-06-07 15:06_

---

_Label `needs-decision` added by @ntBre on 2025-06-07 15:06_

---

_Comment by @ntBre on 2025-06-07 15:13_

This doesn't mean too much since I haven't seen *that* much Python code, but I don't think I've ever seen this pattern before. Is this common? I thought the idiomatic form of these examples would use a `for` loop instead of `while` to handle the iteration automatically.

---

_Comment by @spaceone on 2025-06-07 17:11_

Well, the loop was my demonstration and yes it doesn't make sense. I just used it to show how performance differs.

Yes, the occurrences are few but I saw the `StopIteration` pattern in many projects.

---

_Comment by @spaceone on 2025-06-08 07:16_

As the initial example was irritating I moved the loop into the testit definition.

---

_Comment by @MichaReiser on 2025-06-09 16:43_

We've been hesitant to add new performance-centric rules unless they are inherently slower (e.g., reading an entire file instead of the first few bytes when testing if a file's content starts with a specific character) mainly because performance characteristics can change between CPython versions. The performance is also unlikely to matter unless the code is in a very hot loop, which is hard to tell to determine for Ruff.


Now, the second version is pretty readable. The third version, which is correct in all cases unlike version 2, goes a pretty long way only to avoid `StopIteration`. Which removes the "improved readability" argument IMO. 








---
