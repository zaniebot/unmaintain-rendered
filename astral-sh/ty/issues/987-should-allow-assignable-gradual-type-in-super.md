```yaml
number: 987
title: should allow assignable gradual type in super()
type: issue
state: closed
author: CharlesPerrotMinot
labels:
  - bug
  - typing semantics
assignees: []
created_at: 2025-08-14T15:18:48Z
updated_at: 2025-10-13T10:57:48Z
url: https://github.com/astral-sh/ty/issues/987
synced_at: 2026-01-10T02:06:24Z
```

# should allow assignable gradual type in super()

---

_Issue opened by @CharlesPerrotMinot on 2025-08-14 15:18_

### Summary

ty errors saying the annotation of the `cls` as `type[Any]` for a class inheriting from `type`, in the __init__, is wrong.

We're getting
```
error[invalid-super-argument]: `type[Any]` is not an instance or subclass of `<class 'BuilderMeta'>` in `super(<class 'BuilderMeta'>, type[Any])` call
    |
453 |         dct["use_legacy"] = define_builder_use_legacy()
454 |         dct["dict"] = define_builder_dict()
455 |         instance = super().__new__(cls, name, bases, dct)
    |                    ^^^^^^^
456 |         instance.__field_names__ = field_names
457 |         instance.from_behave_context = define_builder_from_behave_context(instance)
    |
info: rule `invalid-super-argument` is enabled by default
```

Which points to the following code
```
class BuilderMeta(type):
    def __new__(
        cls: type[Any],
        name: str,
        bases: tuple[type, ...],
        dct: dict[str, Any],
    ) -> "BuilderMeta":
        instance = super().__new__(cls, name, bases, dct)
```

Works without issues with mypy 1.17.1, with this config:
```
allow_subclassing_any=true
allow_untyped_decorators=true
ignore_missing_imports = true
no_warn_return_any=true
strict=true
```

### Version

[0.0.1-alpha.18](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.18)

---

_Label `bug` added by @carljm on 2025-08-14 17:13_

---

_Renamed from "invalid-super-argument error when subclassing type" to "should allow assignable gradual type in super()" by @carljm on 2025-08-14 17:13_

---

_Comment by @carljm on 2025-08-14 17:14_

Thanks for the report! Yeah, I think this should be allowed, since `type[Any]` can materialize to `type[BuilderMeta]`, which is valid for this `super()`.

(You can also change the `type[Any]` annotation to `type["BuilderMeta"]`, which is more precise and should be accurate; then [this works in ty](https://play.ty.dev/16d53ebb-fe05-4c64-a4ac-9d449a41052b) today; our problem here is just in not being "forgiving" as we should be with `Any`.)

---

_Label `typing semantics` added by @carljm on 2025-08-14 17:19_

---

_Closed by @AlexWaygood on 2025-10-13 10:57_

---
