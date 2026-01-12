```yaml
number: 22494
title: Range formatting semicolon-separated statements introduces whitespace
type: issue
state: open
author: dylwil3
labels:
  - bug
  - formatter
assignees: []
created_at: 2026-01-10T14:53:50Z
updated_at: 2026-01-12T21:34:27Z
url: https://github.com/astral-sh/ruff/issues/22494
synced_at: 2026-01-12T22:24:39Z
```

# Range formatting semicolon-separated statements introduces whitespace

---

_@dylwil3_

We currently do the following:

```python
class Foo:
    x=1;<RANGE_START>x=2<RANGE_END>
```

becomes:

```python
class Foo:
    x=1;    x = 2
```

The enclosing node of the range here is correctly determined to be the full suite (because it can't find the indentation for `x=2`), but then something must go wrong when narrowing from the formatted

```python
class Foo:
    x=1
    x=2
```

back down. My guess is that this comes down to the [`is_logical_line`](https://github.com/astral-sh/ruff/blob/046c5a46d88c930e4e5e4638ec6b3515ba919982/crates/ruff_python_formatter/src/range.rs#L563) helper being not quite right, or else maybe we aren't emitting/interpreting source positions correctly here? I haven't looked closely.
            

---

_Label `bug` added by @dylwil3 on 2026-01-10 14:54_

---

_Label `formatter` added by @dylwil3 on 2026-01-10 14:54_

---
