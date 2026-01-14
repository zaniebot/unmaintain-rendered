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
updated_at: 2026-01-14T09:08:55Z
url: https://github.com/astral-sh/ruff/issues/22494
synced_at: 2026-01-14T09:34:58Z
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

_Comment by @leonace924 on 2026-01-13 18:12_

Hi @AlexWaygood nice to meet you
I'd like to contribute on this project, is this good issue to take up?
Thank you

---

_Comment by @MichaReiser on 2026-01-14 09:01_

It's probably not the greatest "best first issue" as I don't have a sense for how hard it is to fix this issue and the range formatting code is a bit tricky. But I also don't want to discourage you from giving it a try. Something like https://github.com/astral-sh/ruff/issues/19745 or one off https://github.com/astral-sh/ruff/issues/17203 might be an easier first contribution.

---

_Comment by @leonace924 on 2026-01-14 09:05_

Thank you @MichaReiser, but those issues are already linked to PR...

---

_Comment by @MichaReiser on 2026-01-14 09:08_

https://github.com/astral-sh/ruff/pull/19772 has been stale, it would be great to get a new PR with the feedback addressed

---
