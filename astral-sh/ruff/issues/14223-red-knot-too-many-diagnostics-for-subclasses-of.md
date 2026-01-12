```yaml
number: 14223
title: "[red-knot] Too many diagnostics for subclasses of cyclic classes"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2024-11-09T11:20:14Z
updated_at: 2025-01-20T13:35:31Z
url: https://github.com/astral-sh/ruff/issues/14223
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] Too many diagnostics for subclasses of cyclic classes

---

_@AlexWaygood_

Consider the following Python snippet:

```py
class Foo(Bar): ...
class Bar(Foo): ...
class Baz(Foo): ...
class Spam(Baz): ...
```

If you save this snippet in a `bar.pyi` file and run red-knot over that file, red-knot currently emits four diagnostics:

```
error[cyclic-class-def] /Users/alexw/dev/experiment/bar.pyi:1:1 Cyclic definition of `Foo` or bases of `Foo` (class cannot inherit from itself)
error[cyclic-class-def] /Users/alexw/dev/experiment/bar.pyi:2:1 Cyclic definition of `Bar` or bases of `Bar` (class cannot inherit from itself)
error[cyclic-class-def] /Users/alexw/dev/experiment/bar.pyi:3:1 Cyclic definition of `Baz` or bases of `Baz` (class cannot inherit from itself)
error[cyclic-class-def] /Users/alexw/dev/experiment/bar.pyi:4:1 Cyclic definition of `Spam` or bases of `Spam` (class cannot inherit from itself)
```

Ideally we'd emit the first two of those diagnostics, but not the third or the fourth. Although it's impossible to calculate an accurate MRO for any of these classes due to the cycle between `Foo` and `Bar`, emitting so many diagnostics as a result of a single error is undesirable. If we actually emitted these diagnostics on user code, it would be likely to confuse the user about where the issue actually is.

`Baz` and `Spam` _are_ cyclically defined, of course, and we _do_ need to detect that cycle, as we do currently, or we'll try to calculate the MROs of these classes and fall into infinite recursion. I think what we're currently missing is that after detecting the cycle (or perhaps as part of detecting the cycle), we need to do some extra work somewhere to figure out which classes are actually involved in the cycle. Here it is `Foo` and `Bar`; we should not emit diagnostics for `Baz` and `Spam`, since they are not directly involved in the cycle.

---

_Label `red-knot` added by @AlexWaygood on 2024-11-09 11:20_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-09 11:20_

---

_Label `bug` added by @AlexWaygood on 2024-11-09 12:09_

---

_Closed by @AlexWaygood on 2025-01-20 13:35_

---
