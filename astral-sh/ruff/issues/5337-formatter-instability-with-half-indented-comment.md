---
number: 5337
title: Formatter instability with half-indented comment
type: issue
state: closed
author: konstin
labels:
  - bug
  - good first issue
  - formatter
  - help wanted
assignees: []
created_at: 2023-06-23T16:25:47Z
updated_at: 2023-08-16T07:09:51Z
url: https://github.com/astral-sh/ruff/issues/5337
synced_at: 2026-01-10T01:22:44Z
---

# Formatter instability with half-indented comment

---

_Issue opened by @konstin on 2023-06-23 16:25_

Currently, there is only one case in cpython where the formatter formats something different on the second pass compared to the first pass:

```python
class A:
    def f():
        pass
       # strangely indented comment

print()
```
becomes (trailing comment of `class A`)
```python
class A:
    def f():
        pass
# strangely indented comment


print()
```
becomes (leading comment of `print()`))
```python
class A:
    def f():
        pass


# strangely indented comment


print()
```

The task would be going into `placement.rs` and changing the attachment of that comment so it becomes stable and adding a regression test.

I orginally discovered this in https://github.com/python/cpython/blob/9499b0f138cc53b9a2590350d0b545d2f69ee126/Lib/test/inspect_fodder.py#L96-L120 using
```shell
cargo run --bin ruff_dev check-formatter-stability projects/cpython
```

---

_Label `formatter` added by @konstin on 2023-06-23 16:25_

---

_Label `bug` added by @konstin on 2023-07-16 18:07_

---

_Label `good first issue` added by @konstin on 2023-08-04 09:26_

---

_Label `help wanted` added by @konstin on 2023-08-04 09:27_

---

_Comment by @LaBatata101 on 2023-08-07 15:00_

I would like to work on this.

So, the intended behavior is that after the second pass the code looks like this?

```python
class A:
    def f():
        pass
# strangely indented comment

print()
```

---

_Assigned to @LaBatata101 by @konstin on 2023-08-07 15:25_

---

_Comment by @konstin on 2023-08-08 12:36_

Since the comment is between two levels, it's ok to assign it either to the higher or to the lower level, that is either
```python
class A:
    def f():
        pass
        # strangely indented comment

print()
```
or

```python
class A:
    def f():
        pass
    # strangely indented comment

print()
```
It's important though that the comments gets indented on the same level as the statement it is attached to

---

_Referenced in [astral-sh/ruff#6460](../../astral-sh/ruff/pulls/6460.md) on 2023-08-09 21:14_

---

_Closed by @konstin on 2023-08-11 11:21_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:09_

---
