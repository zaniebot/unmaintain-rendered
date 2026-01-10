```yaml
number: 11117
title: "`PYI026`/`type-alias-without-annotation` false positives with `None` in `Enum`"
type: issue
state: closed
author: Avasam
labels:
  - bug
  - rule
assignees: []
created_at: 2024-04-23T21:10:43Z
updated_at: 2024-04-24T14:48:51Z
url: https://github.com/astral-sh/ruff/issues/11117
synced_at: 2026-01-10T11:09:53Z
```

# `PYI026`/`type-alias-without-annotation` false positives with `None` in `Enum`

---

_Issue opened by @Avasam on 2024-04-23 21:10_

```py
class VariablePolicy(Enum):
    EXPAND_DISTRIBUTED_VARIABLES = "expand_distributed_variables"
    NONE = None
    SAVE_VARIABLE_DEVICES = "save_variable_devices"
```
(taken from https://github.com/python/typeshed/blob/ed7f35a2bc53610d5166cbccbb965fc97cfdbac4/stubs/tensorflow/tensorflow/saved_model/experimental.pyi#L34-L37 )

Running `ruff check --fix --select=PYI026` results in:
```
stubs\tensorflow\tensorflow\saved_model\experimental.pyi:36:5: PYI026 [*] Use `typing_extensions.TypeAlias` for type alias, e.g., `NONE: TypeAlias = None`
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Version: `ruff 0.4.1`

---

_Comment by @AlexWaygood on 2024-04-23 21:27_

Yup. I think we just need to port over the fix I made the other week in https://github.com/PyCQA/flake8-pyi/commit/1279f1e8e496e7a6ce8a83142545f779a57f6d59. Should be pretty simple.

How's your Rust — fancy making a PR? :-) If not, I can do it

---

_Label `bug` added by @AlexWaygood on 2024-04-23 21:27_

---

_Label `rule` added by @AlexWaygood on 2024-04-23 21:27_

---

_Comment by @Avasam on 2024-04-23 22:41_

> How's your Rust

Its in my news cycle, other than that I never touched Rust, but I'm considering it next time I wanna (and have time to) learn a new language.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-04-23 22:52_

---

_Closed by @AlexWaygood on 2024-04-24 14:13_

---
