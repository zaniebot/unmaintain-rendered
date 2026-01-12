```yaml
number: 10231
title: "`FBT003` should not fire on `NamedTuple`s"
type: issue
state: closed
author: tylerlaprade
labels:
  - wontfix
  - rule
assignees: []
created_at: 2024-03-04T21:41:05Z
updated_at: 2024-03-11T17:25:35Z
url: https://github.com/astral-sh/ruff/issues/10231
synced_at: 2026-01-12T15:54:50Z
```

# `FBT003` should not fire on `NamedTuple`s

---

_@tylerlaprade_

```
class Foo(NamedTuple):
    is_bar: bool
    baz_count: int

Foo(True, 3)
```

`FBT003` complains about a positional boolean passed to a function, but the only alternative to initialize this tuple it `Foo(is_bar=True, baz_count=3)`, which defeats the purpose of using a `NamedTuple`.

---

_Label `needs-decision` added by @charliermarsh on 2024-03-05 01:24_

---

_Comment by @charliermarsh on 2024-03-05 01:35_

I'm not sure... To me it's within the scope and intent of the rule. For example, `Foo(True, False, True, 3)` _is_ confusing, and you should be using keyword arguments in calls like that. I personally wouldn't activate this rule, since it's quite strict, but if it's enabled, I don't know that these constructors should be special-cased.


---

_Comment by @AlexWaygood on 2024-03-05 11:02_

> which defeats the purpose of using a `NamedTuple`

Why do you say it defeats the purpose of a namedtuple? I think I might have a different idea to you about what the purpose of a namedtuple is 😃

---

_Comment by @tylerlaprade on 2024-03-05 16:17_

My team was previously using unnamed tuples, so I suggested `NamedTuple` as an upgrade that would still be compatible with all legacy code, but it created friction for them since they had to modify every place one was created rather than just find-and-replace `(a, b, c)` -> `Foo(a, b, c)`.

That being said, I do see both points of view. It's nice to see what each argument represents, although it can feel verbose for tiny tuples that only have one boolean. I encouraged them to do the extra step of adding `a=`, `b=`, and `c=` to their tuples, which they begrudgingly did since I was blocking the PR with raw tuples, and Ruff was blocking it with named tuples with positional arguments.

---

_Comment by @zanieb on 2024-03-11 17:25_

While I agree it could smooth things over during an upgrade, I don't think this makes sense for the spirit of the rule.

---

_Closed by @zanieb on 2024-03-11 17:25_

---

_Label `needs-decision` removed by @zanieb on 2024-03-11 17:25_

---

_Label `wontfix` added by @zanieb on 2024-03-11 17:25_

---

_Label `rule` added by @zanieb on 2024-03-11 17:25_

---
