```yaml
number: 2445
title: "`types.ModuleType` attributes in the global namespace can have a different type rendered in the autocomplete tooltip to the one ty infers"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2026-01-11T13:41:14Z
updated_at: 2026-01-12T00:38:00Z
url: https://github.com/astral-sh/ty/issues/2445
synced_at: 2026-01-12T02:26:12Z
```

# `types.ModuleType` attributes in the global namespace can have a different type rendered in the autocomplete tooltip to the one ty infers

---

_Issue opened by @AlexWaygood on 2026-01-11 13:41_

### Summary

For example, here the inferred type is `str`, but the type the autocompletion tooltip renders for the attribute is `str | None`:

<img width="1208" height="300" alt="Image" src="https://github.com/user-attachments/assets/77946354-9824-4776-8053-0ef602cd40b4" />

---

What's going on here is: in general, ty knows that the `__file__` attribute for a module will be of type `str | None`. But for _this_ module, ty infers a more precise type (`str`), because, well: we know this module has a file; we're checking it right now.

This special-casing is done [here](https://github.com/astral-sh/ruff/blob/2c68057c4b0a666ffd73324013fad2ce3c2fa547/crates/ty_python_semantic/src/place.rs#L1640-L1708); it looks like we might need to make our autocompletion machinery aware of that special-casing somehow?

### Version

_No response_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-11 13:41_

---

_Label `bug` added by @AlexWaygood on 2026-01-11 13:41_

---

_Label `server` added by @AlexWaygood on 2026-01-11 13:41_

---

_Label `completions` added by @AlexWaygood on 2026-01-11 13:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-12 00:38_

---
