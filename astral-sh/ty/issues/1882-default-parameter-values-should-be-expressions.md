```yaml
number: 1882
title: Default parameter values should be expressions, not types
type: issue
state: open
author: hamdanal
labels:
  - bug
assignees: []
created_at: 2025-12-14T13:36:07Z
updated_at: 2025-12-18T00:47:50Z
url: https://github.com/astral-sh/ty/issues/1882
synced_at: 2026-01-10T01:53:59Z
```

# Default parameter values should be expressions, not types

---

_Issue opened by @hamdanal on 2025-12-14 13:36_

### Summary

Notice the default values of `Literal[False]` instead of `False`. Also `…`, which is a very common default parameter value in stub files, is rendered as `EllipsisType`.

<img width="461" height="221" alt="Image" src="https://github.com/user-attachments/assets/104b7f2c-5a96-4706-b28a-d34c094ce300" />

### Version

Latest VSCode extension and ty version 0.0.1-alpha.34

---

_Label `bug` added by @MichaReiser on 2025-12-14 13:44_

---

_Label `server` added by @MichaReiser on 2025-12-14 13:44_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-14 13:44_

---

_Comment by @AlexWaygood on 2025-12-14 13:45_

Thanks! I agree that this is confusing and we should change this. We didn't have an issue before now for it though, so this is useful.

There's some design work we need to do to decide exactly what heuristics to use when rendering default values as _values_ rather than _types_. There was some discussion in https://github.com/astral-sh/ruff/pull/20991#issuecomment-3426434202 and the following comments.

---

_Comment by @AlexWaygood on 2025-12-14 13:46_

@MichaReiser I don't just see this as a server issue FWIW — we have the same confusing display in our diagnostics emitted from the `ty_python_semantic` crate 

---

_Label `server` removed by @MichaReiser on 2025-12-14 13:50_

---

_Assigned to @Gankra by @Gankra on 2025-12-15 17:32_

---

_Comment by @Gankra on 2025-12-16 16:37_

I took a quick super-naive crack at this but hit a bit of a road-block on dataclasses:

```py
from dataclasses import dataclass, field

@dataclass
class Member:
    name: str
    role: str = field(default="user")
    tag: str | None = field(default=None, init=False)

# revealed: (self: Member, name: str, role: str = Literal["user"]) -> None
reveal_type(Member.__init__)
```

In this case the default type/expression comes from some deeper recursive analysis, and needs to be bubbled up. I'll try to dig through it more later.

---

_Comment by @AlexWaygood on 2025-12-16 16:44_

For anything complex (and I'd include "generated methods that don't have source code" in that bucket), I think we can just render the parameter default as `...`. That's still less confusing than what we have currently, I think, and matches pylance.

For me the more interesting questions are things like:
- How many elements can a list/dict/set/tuple literal have before we fall back to `...`?
- How "deep" should containers (lists/sets/dicts) be before we decide they're "too complex" and fall back to `...`? (E.g. if the default is something like `[[[[[[[[[], [], (1, 2, 3), (4, 5, 6), {}], {}, ], {}, ]]]]]]]` or whatever, the list only has one item but that's clearly not something we should render as part of a bigger signature display (IMO).
- What heuristics should we use to decide when numbers should be rendered in octal? In hexadecimal? Do we just want to copy what the source code does? Pylance does a poor job here IMO.
- Should we just render all attribute expressions as they appear in the source code? Typeshed uses `x: int = sys.maxsize` a lot; it would be a shame to have all those just rendered as `x: int = ...`, the way pylance does.

---

_Closed by @Gankra on 2025-12-16 18:39_

---

_Comment by @AlexWaygood on 2025-12-16 18:40_

reopening. https://github.com/astral-sh/ruff/pull/22010 gets us to a much better state but I think there's still lots of improvements we can and should make to get to parity with Pylance here (and I have dreams of doing better than Pylance in this area).

---

_Reopened by @AlexWaygood on 2025-12-16 18:40_

---

_Label `server` added by @carljm on 2025-12-16 18:44_

---

_Renamed from "Default parameter values are formatted as types in hover tooltips" to "Default parameter values should be expressions, not types" by @Gankra on 2025-12-16 19:10_

---

_Comment by @Gankra on 2025-12-16 19:11_

rebranding the issue to be about things like `x=sys.maxsize`, `x = [1, 2, 3]`, etc.

---

_Label `server` removed by @AlexWaygood on 2025-12-16 19:12_

---

_Closed by @AlexWaygood on 2025-12-17 10:21_

---

_Reopened by @Gankra on 2025-12-17 15:34_

---

_Unassigned @Gankra by @Gankra on 2025-12-18 00:47_

---
