```yaml
number: 1480
title: Time Machine pytest fixture hidden behind conditional
type: issue
state: open
author: mzealey
labels:
  - configuration
  - wish
assignees: []
created_at: 2025-11-05T09:43:09Z
updated_at: 2025-11-05T21:45:16Z
url: https://github.com/astral-sh/ty/issues/1480
synced_at: 2026-01-12T15:54:25Z
```

# Time Machine pytest fixture hidden behind conditional

---

_@mzealey_

### Summary

Somehow mypy makes this work, but you can see that https://github.com/adamchainz/time-machine/blob/main/src/time_machine/__init__.py#L409 is hidden behind an `if HAVE_PYTEST` conditional, meaning that it is mentioned by `ty` as a `possibly-missing-attribute`.

### Version

_No response_

---

_Label `configuration` added by @sharkdp on 2025-11-05 20:05_

---

_Label `wish` added by @sharkdp on 2025-11-05 20:05_

---

_Comment by @sharkdp on 2025-11-05 20:07_

I don't think mypy has any special handling here, it simply doesn't keep track of the fact that the module attribute might actually not be defined. And you don't see the "undefined symbol" error on `if HAVE_PYTEST` because it's a 3rd party dependency?

mypy *does* have a feature where you can configure that it should assume certain variables/constants to exist (e.g. https://mypy.readthedocs.io/en/stable/config_file.html#confval-always_true). We don't have such a feature, but can certainly consider adding this, such that users could specify a list of variables that ty should consider being set to `True`/`False` (similar to how `TYPE_CHECKING` is treated in a special way).

---

_Comment by @carljm on 2025-11-05 20:40_

I think there are several possible things we could do to improve this scenario, roughly ranked from easiest to hardest:

1. Make the "possibly undefined import" diagnostic disabled by default, because it is prone to this kind of false positive. (Same rationale for why we already disabled the "possibly undefined local variable" diagnostic.) I don't know of any other type checker that offers this diagnostic?
2. Add the feature @sharkdp mentions, so the user can just explicitly configure that `HAVE_PYTEST` should always be considered `True`.
3. Add sophisticated handling of `try: import ...` / `except ImportError:` such that we consider it a "statically known branch" based on the known presence or absence of the module in our environment.

---

_Comment by @sharkdp on 2025-11-05 21:44_

> Add sophisticated handling of `try: import ...` / `except ImportError:` such that we consider it a "statically known branch" based on the known presence or absence of the module in our environment.

I've been thinking about this recently, and for what it's worth, we sort-of support this today. If you can somehow turn something that is either of type `<module 'xyz'>` or of type `Unknown` into a `Literal[True]` / `Literal[False]`, you can define this `HAS_XYZ` constant and use it to conditionally hide parts of your code, if the optional dependency `xyz` is not available. I'm not saying we should suggest that users use *this exact* pattern, but as a demo, this is how this can work today:

```py
try:
    import xyz  # type: ignore
except ImportError:
    pass

HAS_XYZ = bool(is_subtype_of(TypeOf[xyz], ModuleType))
```

And here is a playground that shows how this can be used to conditionally add an overload, if a given optional dependency is available (something I discussed with @MichaReiser the other day):

https://play.ty.dev/c4512390-e8c2-4cd8-acd6-a160b42c01f3

(try removing/renaming the `xyz.py` tab to simulate the situation in which that dependency is not available)

---
