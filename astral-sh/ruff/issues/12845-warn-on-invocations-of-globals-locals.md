```yaml
number: 12845
title: "warn on invocations of `globals()` / `locals()`"
type: issue
state: open
author: kiaradlf
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-08-12T12:40:53Z
updated_at: 2024-08-19T08:16:39Z
url: https://github.com/astral-sh/ruff/issues/12845
synced_at: 2026-01-12T15:54:52Z
```

# warn on invocations of `globals()` / `locals()`

---

_@kiaradlf_

python has a number of introspection functions, among which [`globals()`](https://docs.python.org/3/library/functions.html#globals) and [`locals()`](https://docs.python.org/3/library/functions.html#locals).

these can currently be used both for reading of variables as well as for assignment purposes, say:

```python
for foo in foos:
  globals()[foo] = ...
```

while it seems that ruff currently lets these slide, i believe that for both legibility as well as to facilitate static analysis, use of such invocations is considered an anti-pattern.
it would be nice to see ruff suggest users to reconsider using the likes of these.


---

_Renamed from "warn on invocations of `globals()`" to "warn on invocations of `globals()` / `locals()`" by @kiaradlf on 2024-08-12 12:41_

---

_Comment by @Avasam on 2024-08-19 05:28_

Relates to https://github.com/astral-sh/ruff/issues/2031#issuecomment-2294987091 as well

---

_Label `needs-decision` added by @MichaReiser on 2024-08-19 08:10_

---

_Label `rule` added by @MichaReiser on 2024-08-19 08:10_

---

_Comment by @MichaReiser on 2024-08-19 08:16_

That seems a reasonable "restriction" rule to me. Another relevant rule for `locals` seems to lint against mutations on `assignments` (although that's also something a type checker is good at catching).

I'm leaning towards deferring the implementation of such a rule until #1774 is done because there are valid use cases for both expressions. (@AlexWaygood what's your take?)

---
