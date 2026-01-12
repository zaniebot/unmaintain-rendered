```yaml
number: 447
title: invalid-exception-caught false positive with aliased error
type: issue
state: closed
author: sk-
labels:
  - bug
  - help wanted
  - type properties
assignees: []
created_at: 2025-05-19T12:15:11Z
updated_at: 2025-05-19T21:15:51Z
url: https://github.com/astral-sh/ty/issues/447
synced_at: 2026-01-12T15:54:23Z
```

# invalid-exception-caught false positive with aliased error

---

_@sk-_

### Summary

When a base exception class is aliased to a variable, ty seems to forget its original type and incorrectly raises `invalid-exception-caught`.

An example can be see with the following [playground](https://play.ty.dev/8d3b8aec-8d56-470b-92ee-ce521ae3b6d4) code

```python
from compat import BASE_EXCEPTION_CLASS
# If the variable is defined in the same module, no error is raised
# BASE_EXCEPTION_CLASS = Exception

class Error(BASE_EXCEPTION_CLASS):
    pass

try:
    {}.get("foo")
except Error as err:
    pass
```

which produces the error

```
Cannot catch object of type `<class 'Error'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses) (invalid-exception-caught) [Ln 9, Col 8]
```

### Version

ty 0.0.1-alpha.5 (3b726d87a 2025-05-17) and playground

---

_Label `bug` added by @AlexWaygood on 2025-05-19 12:18_

---

_Comment by @sharkdp on 2025-05-19 12:19_

Thank you for reporting this.

Background: When you import an *undeclared* symbol from another module, we union its type with `Unknown`. In your case, `BASE_EXCEPTION_CLASS` has the type `Unknown | <class 'Exception'>`. The rationale for this is explained in [this document](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md) (and more information [here](https://github.com/astral-sh/ruff/blob/b86960f18c010ef991bc91e93bcff84b44a539fd/crates/ty_python_semantic/resources/mdtest/boundness_declaredness/public.md)). To prevent this from happening, you can declare the symbol. This is currently difficult, as we don't support `typing.Final` or `typing.TypeAlias`, yet.

In any case, we should probably attempt to silence the `invalid-exception-caught` diagnostic in this case, I think.

---

_Comment by @AlexWaygood on 2025-05-19 12:25_

I think the root cause here might be an assignability bug: a class we infer as having `Unknown` in its MRO should be inferred as being assignable to `BaseException` (since the `Unknown` could materialise to `BaseException`). So this check here should be passing, but it seems like it isn't: https://github.com/astral-sh/ruff/blob/220137ca7bfd9a0c9178ae21d00737d932d21af8/crates/ty_python_semantic/src/types/infer.rs#L2605

---

_Label `type properties` added by @AlexWaygood on 2025-05-19 12:25_

---

_Comment by @sharkdp on 2025-05-19 12:39_

Yes, that's [correct](https://play.ty.dev/5b95ff47-ec5c-479e-8678-96b6ce06f5a9).

---

_Label `help wanted` added by @sharkdp on 2025-05-19 14:48_

---

_Closed by @sharkdp on 2025-05-19 21:15_

---
