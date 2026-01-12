```yaml
number: 538
title: invalid-return-type error message is too weak for wrong return type
type: issue
state: closed
author: konstin
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-05-28T12:21:15Z
updated_at: 2025-05-30T00:20:17Z
url: https://github.com/astral-sh/ty/issues/538
synced_at: 2026-01-12T15:54:23Z
```

# invalid-return-type error message is too weak for wrong return type

---

_@konstin_

### Summary

Playground link: https://play.ty.dev/95591965-0266-4a59-9952-4e8be99ee67c

```python
from pathlib import Path

def setup_test_project(registry_name: str, registry_url: str, project_dir: str) -> Path:
    pyproject_file = Path(project_dir) / "pyproject.toml"
    pyproject_file.write_text("...", encoding="utf-8")
```

The error to me implies that I need to use `None | Path` when the function can never return `Path` at all and I should replaced `Path` with `None` instead

```
error[invalid-return-type]: Function can implicitly return `None`, which is not assignable to return type `Path`
  --> script.py:64:84
   |
64 | def setup_test_project(registry_name: str, registry_url: str, project_dir: str) -> Path:
   |                                                                                    ^^^^
65 |     pyproject_file = Path(project_dir) / "pyproject.toml"
66 |     pyproject_file.write_text("...", encoding="utf-8")
   |
info: rule `invalid-return-type` is enabled by default
```

### Version

ty 0.0.1-alpha.7

---

_Label `diagnostics` added by @konstin on 2025-05-28 12:21_

---

_Comment by @AlexWaygood on 2025-05-28 12:46_

How about if there are 0 `return` statements in the function (or if the function _only_ has bare `return` statements rather than `return <variable>` statements) we change the message to:

```
Function always implicitly returns `None`, which is not assignable to return type `Path`
```

And add a subdiagnostic that says:

```
note: Consider changing your return annotation to `-> None`
```

---

_Comment by @konstin on 2025-05-28 12:47_

Hinting at that would be even better, this case usually happens when someone forgot a `return`, or as happened here, didn't change the return type after removing the `return`.

---

_Label `help wanted` added by @AlexWaygood on 2025-05-28 15:36_

---

_Comment by @lipefree on 2025-05-28 20:22_

Hey! I may have something worth checking out that implements this, I will do a PR!

---

_Comment by @carljm on 2025-05-29 00:35_

> this case usually happens when someone forgot a `return`, or as happened here, didn't change the return type after removing the `return`.

These do seem like the two likely reasons for this case, but hinting to change the return type to `-> None` is only the right hint for the latter case, it's the wrong hint for the former case (instead you'd want to add a `return` statement.) Is there a reason why we think the latter case is more likely?

ETA: There is of course a third possibility, which is that `Path | None` would be the right annotation (e.g. maybe this is a placeholder method on a base class that always returns `None`, but subclasses might return `Path`).

Personally I don't really see the current diagnostic as pushing any one of those three possibilities; it's purely stating the observable facts. If we make this change, we are saying we think the first two possibilities (or just the second one) are more likely than the third one. This is probably true.

---

_Comment by @mtshiba on 2025-05-29 10:45_

One thing we can consider, other than the number of `return`s in a function body, is that if the inferred type of the last expression in the function body is compatible with the annotated return type, the user is likely to have forgotten to type `return` (although this may not be very reliable if the result type is a gradual type).


---

_Comment by @AlexWaygood on 2025-05-29 10:46_

> These do seem like the two likely reasons for this case, but hinting to change the return type to `-> None` is only the right hint for the latter case, it's the wrong hint for the former case (instead you'd want to add a `return` statement.) Is there a reason why we think the latter case is more likely?

No; I agree we should give both hints. I already thumbsed-up Konsti's comment in https://github.com/astral-sh/ty/issues/538#issuecomment-2916186994 saying the same thing! 😄

---

_Closed by @carljm on 2025-05-30 00:20_

---
