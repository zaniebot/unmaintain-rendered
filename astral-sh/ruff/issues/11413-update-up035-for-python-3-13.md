```yaml
number: 11413
title: Update UP035 for Python 3.13
type: issue
state: closed
author: AlexWaygood
labels:
  - rule
  - python313
assignees: []
created_at: 2024-05-13T17:08:59Z
updated_at: 2024-06-02T21:59:49Z
url: https://github.com/astral-sh/ruff/issues/11413
synced_at: 2026-01-10T11:09:53Z
```

# Update UP035 for Python 3.13

---

_Issue opened by @AlexWaygood on 2024-05-13 17:08_

We should update UP035 to emit a diagnostic if `--target-version=py313` has been selected and a user imports `typing.TypeIs`, `warnings.deprecated`, `typing.ReadOnly`, `typing.NoDefault` `typing.get_protocol_members` or `typing.is_protocol` from `typing_extensions` rather than `typing`

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/issues/11411#issuecomment-2108265396_

---

_Label `rule` added by @AlexWaygood on 2024-05-13 17:09_

---

_Comment by @AlexWaygood on 2024-05-13 17:17_

Other updates that are required
- We should emit an error if `TypeVar`, `ParamSpec` or `TypeVarTuple` is imported from `typing_extensions` and the script targets py313+ (but not if the script targets lower versions)
- We should _not_ emit an error if the script targets py312 or lower, and any of `TypedDict`, `Protocol`, `runtime_checkable` or `get_type_hints` is imported from `typing_extensions`. These all have their implementations from Python 3.13 backported in `typing_extensions`. (We currently do emit errors for all of these; this is a false positive.)

---

_Comment by @AlexWaygood on 2024-05-13 17:21_

Lastly, for `(Async)Generator` and `(Async)ContextManager`, we should recommend for the user to import them from `collections.abc` or `contextlib` rather than `typing_extensions` on Python 3.9-3.12 (and not emit a diagnostic at all on Python <=3.8). Currently if we see a user importing then from `typing_extensions`, we tell the user to import them from `typing` instead, but that's no longer a good suggestion. The `typing_extensions` versions backport the `typing` versions from Python 3.13, but on Python 3.9-3.12, the `collections.abc`/`contextlib` versions should still be a dropin replacement.

---

_Label `python313` added by @MichaReiser on 2024-05-27 07:54_

---

_Closed by @AlexWaygood on 2024-06-02 21:59_

---
