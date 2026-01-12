```yaml
number: 18686
title: "F401 autofix (adding import to `__all__`) should be unsafe"
type: issue
state: open
author: robsdedude
labels: []
assignees: []
created_at: 2025-06-15T16:37:54Z
updated_at: 2025-06-16T17:32:59Z
url: https://github.com/astral-sh/ruff/issues/18686
synced_at: 2026-01-12T15:54:56Z
```

# F401 autofix (adding import to `__all__`) should be unsafe

---

_@robsdedude_

### Summary

The docs state

> ### [Fix safety](https://docs.astral.sh/ruff/linter/#fix-safety)
>
> Ruff labels fixes as "safe" and "unsafe". The meaning and intent of your code will be retained when applying safe fixes, but the meaning could change when applying unsafe fixes.

`F401`'s autofix will add unused (first-party) imports to `__all__` and consider that a safe fix. I argue it's unsafe. It caught me multiple times off-guard where I just forgot to also delete the import after restructuring a package.

```python
from ._foo import BarOld as _BarOld
from ._foo import Bar

__all__ = [
    "Bar",
]

def __getattr__(name):
    if name in {"BarOld"}:  # actual code has more than just one name here
        import warnings
        warnings.warn("don't", category=DeprecationWarning, stack_level=2)
        return globals[f"_{name}"]
```
I'm absolutely fine with ruff not understanding the `globals` magic and am happy to slap a `noqa: F401` on the import, that's not the issue.
But exposing `_BarOld` (as the fix adds it to `__all__` is really not my intention or what I meant. So for such use-cases, the fix is not safe.

Even if I were to omit the `globals` access and spell out `return _BarOld` instead, it's fine for now until I decide to turn the deprecation into a removal. Removing the branch in `__getattr__` will leave me again with the same fix that's considered safe, whereas in reality I've just forgotten to remove the import.

---

_Comment by @ntBre on 2025-06-15 17:12_

Hmm, I wonder if we should avoid adding imports with an underscore prefix to `__all__` in general. Otherwise I think it could make sense just to make the fix always unsafe in `__init__.py` files. There's currently some pretty complicated logic for deciding if it's safe or not:

https://github.com/astral-sh/ruff/blob/e4423044f8abc139b33f8f56061d03692ca2cc5f/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs#L279-L290

We also mentioned skipping a related rule in the presence of a `__getattr__` function recently, which could be relevant here too: https://github.com/astral-sh/ruff/issues/18504.

---

_Comment by @MichaReiser on 2025-06-16 06:25_

Isn't this less about fix safety but more that we can't tell what the user intentions are in case of an unused import with an `__all__`? The import could be a leftover from a refactor and now indeed be unused or it was intended to be exportet and should be added to `__all__`. But Ruff simply can't know and shouldn't provide safe fix.

---

_Comment by @ntBre on 2025-06-16 14:20_

> Isn't this less about fix safety but more that we can't tell what the user intentions are in case of an unused import with an `__all__`? The import could be a leftover from a refactor and now indeed be unused or it was intended to be exportet and should be added to `__all__`. But Ruff simply can't know and shouldn't provide safe fix.

Yeah, I think that's right. It's ambiguous what the user intended. Did you mean we shouldn't provide a fix at all there at the end? I think that could make sense too, or maybe a display-only fix that could be applied in an editor.

---

_Comment by @MichaReiser on 2025-06-16 17:16_

I mainly changed the reasoning for the fix safety. I don't think it's not safe because it breaks code in anyway. It's mainly that ruff doesn't know what the user wants. 

I don't remember. Are manual fixes shown in vs code?

---

_Comment by @ntBre on 2025-06-16 17:32_

Oh I see what you mean, I misunderstood.

And yes, it looks like `DisplayOnly` fixes are shown in VS Code. I tested with UP049:

https://github.com/astral-sh/ruff/blob/c5b58187da1809c778d886595ec50f7db68fd3b9/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs#L156-L163

![Image](https://github.com/user-attachments/assets/be10c114-cc55-4599-b1aa-5fc66869096e)

---
