---
number: 2188
title: Improve predictability of instance attribute type inference
type: issue
state: open
author: wvanbergen
labels:
  - narrowing
  - control flow
assignees: []
created_at: 2025-12-23T17:12:55Z
updated_at: 2025-12-27T06:50:11Z
url: https://github.com/astral-sh/ty/issues/2188
synced_at: 2026-01-10T01:51:14Z
---

# Improve predictability of instance attribute type inference

---

_Issue opened by @wvanbergen on 2025-12-23 17:12_

### Summary

Unrelated code in a  initializer appears to prevent type narrowing. Example:

``` python
class Foo:
    def bar(self) -> None:
        # revealed type changes to `Literal[True]` if you comment out any of these lines related to _unrelated
        if hasattr(self, "_unrelated"):
            return
        self._unrelated = True

        self._b: bool | None = None    
        self._b = True
        reveal_type(self._b)  # bool | None, expected Literal[True]
```

If you comment out any of the `_unrelated` lines, it changes to `Literal[True]`.

This behaviour seems to be specific to it being an instance method. For a regular function, the type narrowing never happens.

- Playground recreation: https://play.ty.dev/c43a9722-ed99-4749-92d3-ada996276c66
- mypy evaluates it as `builtins.bool`: https://mypy-play.net/?mypy=latest&python=3.12&gist=a2789d2ba9c6b0c511c577bba939e150
- Running `uvx ty check`, no `ty` related settings in `pyproject.toml`

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Comment by @carljm on 2025-12-23 19:58_

Heh, file this one under "things I did not expect to get an issue about." Lesson learned: everything will be noticed :) Thanks for the report!

Your "unrelated" code is not really entirely unrelated, because of the conditional `return`. What happens here is that we enter a cycle finding the attributes of instances of `Foo`, because we flip between thinking the class has two attributes and thinking it has none (because once we think it has `_unrelated`, then we see the early exit.) We fallback to just going with the declared type, which I think is reasonable.

There are various possible improvements here, though I don't think the current behavior is that much of a problem.

---

_Renamed from "Unrelated code in the class instance method appears to prevent type narrowing" to "Improve predictability of instance attribute type inference" by @carljm on 2025-12-23 19:59_

---

_Label `narrowing` added by @mtshiba on 2025-12-27 06:50_

---

_Label `control flow` added by @mtshiba on 2025-12-27 06:50_

---
