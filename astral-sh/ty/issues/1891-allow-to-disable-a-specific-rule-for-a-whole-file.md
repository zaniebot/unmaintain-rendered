```yaml
number: 1891
title: Allow to disable a specific rule for a whole file
type: issue
state: open
author: lypwig
labels:
  - configuration
  - suppression
assignees: []
created_at: 2025-12-15T15:55:32Z
updated_at: 2025-12-30T02:22:18Z
url: https://github.com/astral-sh/ty/issues/1891
synced_at: 2026-01-10T01:56:41Z
```

# Allow to disable a specific rule for a whole file

---

_Issue opened by @lypwig on 2025-12-15 15:55_

### Summary

To disable a rule on a line, I could do:

```py
foo() # ty: ignore[call-non-callable]
```

I can also disable type checking on the whole file by adding on the beginning of the file:

```py
# type: ignore
```

It could be very practical allow a mix of both behaviors: disabling a specific rule for a whole file, by adding on the beginning of the file:

```py
# ty: ignore[call-non-callable]
```

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-12-15 16:00_

I'd support adding this feature. Pyright has it, and I find it useful: https://microsoft.github.io/pyright/#/comments?id=file-level-type-controls

---

_Label `configuration` added by @AlexWaygood on 2025-12-15 16:00_

---

_Label `suppression` added by @AlexWaygood on 2025-12-15 16:00_

---

_Comment by @MichaReiser on 2025-12-15 16:55_

We probably want to align with Ruff that supports both block and [file-level](https://docs.astral.sh/ruff/linter/#file-level) suppressions

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-15 16:55_

---

_Comment by @lypwig on 2025-12-16 08:52_

Just in case, I also want to add that using the syntax `# type: ignore[call-non-callable]` (or anything starting with `# type: ignore`) at the beginning will disable Pyright, which is not a desirable behavior.

---

_Comment by @nathanscain on 2025-12-30 02:21_

> We probably want to align with Ruff that supports both block and [file-level](https://docs.astral.sh/ruff/linter/#file-level) suppressions

chiming in to support block level suppressions particularly for single class constructions or function calls.

I prefer

```
# ty: ignore[missing-typed-dict-key]
return MyTypedDict(
    **another_typed_dict_instance,
    ...
)
```

over

```python
return MyTypedDict(  # ty: ignore[missing-typed-dict-key]
    **another_typed_dict_instance,
    ...
)
```

also, I like

```python
# ty: ignore[some-arg-rule]
function_name(
    a,
    b,
    c,
    ...
)
```

over

```python
function_name(
    a,  # ty: ignore[some-arg-rule]
    b,  # ty: ignore[some-arg-rule]
    c,  # ty: ignore[some-arg-rule]
    ...
)
```



---
