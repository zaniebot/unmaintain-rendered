```yaml
number: 18202
title: "[ty] Raise `invalid-exception-caught` even when exception is not captured"
type: pull_request
state: merged
author: adamaaronson
labels:
  - ty
assignees: []
merged: true
base: main
head: adam/uncaptured-invalid-exception
created_at: 2025-05-19T17:34:19Z
updated_at: 2025-05-19T22:14:02Z
url: https://github.com/astral-sh/ruff/pull/18202
synced_at: 2026-01-12T15:56:14Z
```

# [ty] Raise `invalid-exception-caught` even when exception is not captured

---

_@adamaaronson_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

Closes https://github.com/astral-sh/ty/issues/448

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix bug where `invalid-exception-caught` is not raised on invalid exceptions without explicit capturing (i.e. not using the `as` keyword) by creating a new `infer_exception` method that is called when inferring exceptions both with and without a definition.

## Test Plan

<!-- How was it tested? -->

Added new tests to `exception/basic.md` (for basic exceptions) and `exception/except_star.md` (for star exceptions) to ensure that `invalid-exception-caught` is properly raised even when the exception is not explicitly captured.

---

_Review requested from @carljm by @adamaaronson on 2025-05-19 17:34_

---

_Review requested from @AlexWaygood by @adamaaronson on 2025-05-19 17:34_

---

_Review requested from @sharkdp by @adamaaronson on 2025-05-19 17:34_

---

_Review requested from @dcreager by @adamaaronson on 2025-05-19 17:34_

---

_Comment by @github-actions[bot] on 2025-05-19 17:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
stone (https://github.com/dropbox/stone)
+ error[invalid-exception-caught] stone/backends/python_rsrc/stone_validators.py:714:16: Cannot catch object of type `list[Unknown]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 178 diagnostics
+ Found 179 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psbsd.py:381:20: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psbsd.py:381:20: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psbsd.py:509:12: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psbsd.py:511:12: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psosx.py:252:16: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psosx.py:321:16: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psosx.py:323:16: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_psosx.py:363:20: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_pssunos.py:472:16: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_pssunos.py:482:16: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[invalid-exception-caught] ddtrace/vendor/psutil/_pswindows.py:783:20: Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 7002 diagnostics
+ Found 7013 diagnostics

```
</details>


---

_Label `ty` added by @MichaReiser on 2025-05-19 17:48_

---

_Comment by @adamaaronson on 2025-05-19 18:10_

This surfaces another bug, when ty is run on:

```python
def name_1[name_0: name_0](name_5: name_0):
    try:
        pass
    except name_5:
        pass
```

It triggers a fatal error on `infer.rs:2601`, raising:

```
`Type::to_instance()` should always return `Some()` if called on a type assignable to `type[BaseException]`
```

It looks  like `<name_5 element>.is_assignable_to(self.db type_base_exception)` is resolving to `true`, even though `name_5` is an arbitrary variable type. This error wasn't surfaced before because uncaptured exception types weren't type checked at all.

cc @AlexWaygood @dcreager 

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-19 18:49_

---

_Comment by @AlexWaygood on 2025-05-19 18:50_

> This surfaces another bug

This was a bit subtle! Your PR ended up exposing a latent bug in our implementation of `Type::to_instance()` for `TypeVar` types. I fixed it in https://github.com/astral-sh/ruff/pull/18202/commits/88ebd9f96bb332d1f3319020a813eae3a5ef9abb!

---

_@AlexWaygood approved on 2025-05-19 21:46_

---

_Merged by @AlexWaygood on 2025-05-19 22:13_

---

_Closed by @AlexWaygood on 2025-05-19 22:13_

---

_Comment by @AlexWaygood on 2025-05-19 22:14_

Thank you @adamaaronson!! Sorry for the complications of discovering latent bugs that were not at all caused by your PR!

---
