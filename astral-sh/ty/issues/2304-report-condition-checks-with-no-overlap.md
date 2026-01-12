```yaml
number: 2304
title: Report condition checks with no overlap
type: issue
state: open
author: CoderJoshDK
labels:
  - wish
  - lint
assignees: []
created_at: 2026-01-02T17:30:44Z
updated_at: 2026-01-06T01:17:07Z
url: https://github.com/astral-sh/ty/issues/2304
synced_at: 2026-01-12T15:54:26Z
```

# Report condition checks with no overlap

---

_@CoderJoshDK_

ty does not emit a warning or error when doing a logical comparison where there is no overlap

https://play.ty.dev/beedfd79-499c-443f-a083-4c874e4b5350
```py
foo: str = "bar"

if foo is not None:
    a = "hit"
    print("hit")

if foo is None:
    a = "miss"
    print("miss")

reveal_type(a)
```

basedpyright does provide `1. Condition will always evaluate to True since the types "Literal['bar']" and "None" have no overlap [reportUnnecessaryComparison]`

mypy does not report anything and same with pyright

Another interesting quirk from the playground is that ty knows that the second condition check is unreachable and will then report a as
`1. Revealed type: `Literal["hit"]` [revealed-type]`
But funny enough, basedpyright reports it as `1. Type of "a" is "Literal['miss', 'hit']"`

---

Emitting a warning when there is no overlap between checks feels like an amazing rule to have! 

Chain that brought this issue on https://github.com/rotki/rotki/pull/11269/ and https://discord.com/channels/1039017663004942429/1279201882337705994/1456681723470418123

---

_Comment by @AlexWaygood on 2026-01-02 17:35_

Interesting! I would have expected mypy to flag the first condition with `--strict-equality --enable-error-code=redundant-expr`, but even with those flags added it doesn't complain about it. (Mypy _does_ complain about the second condition if you add `--warn-unreachable`.)

---

_Label `wish` added by @AlexWaygood on 2026-01-02 17:35_

---

_Label `lint` added by @AlexWaygood on 2026-01-02 17:35_

---

_Comment by @AlexWaygood on 2026-01-02 17:36_

This is related to https://github.com/astral-sh/ty/issues/576

---

_Comment by @carljm on 2026-01-06 01:17_

I think it's also related to #1948 

---
