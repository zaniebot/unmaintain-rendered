```yaml
number: 21475
title: "[ty] Improve diagnostics for invalid exceptions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/raise-notimplemented
created_at: 2025-11-15T19:44:31Z
updated_at: 2025-11-15T22:12:01Z
url: https://github.com/astral-sh/ruff/pull/21475
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Improve diagnostics for invalid exceptions

---

_Pull request opened by @AlexWaygood on 2025-11-15 19:44_

## Summary

Not a high-priority task... but it _is_ a weekend :P

This PR improves our diagnostics for invalid exceptions. Specifically:
- We now give a special-cased ``help: Did you mean `NotImplementedError`` subdiagnostic for `except NotImplemented`, `raise NotImplemented` and `raise <EXCEPTION> from NotImplemented`
- If the user catches a tuple of exceptions (`except (foo, bar, baz):`) and multiple elements in the tuple are invalid, we now collect these into a single diagnostic rather than emitting a separate diagnostic for each tuple element
- The explanation of why the `except`/`raise` was invalid ("must be a `BaseException` instance or `BaseException` subclass", etc.) is relegated to a subdiagnostic. This makes the top-level diagnostic summary much more concise.

## Test Plan

Lots of snapshots. And here's some screenshots:

<details>
<summary>Screenshots</summary>

<img width="1770" height="1520" alt="image" src="https://github.com/user-attachments/assets/7f27fd61-c74d-4ddf-ad97-ea4fd24d06fd" />

<img width="1916" height="1392" alt="image" src="https://github.com/user-attachments/assets/83e5027c-8798-48a6-a0ec-1babfc134000" />

<img width="1696" height="588" alt="image" src="https://github.com/user-attachments/assets/1bc16048-6eb4-4dfa-9ace-dd271074530f" />

</details>


---

_Label `ty` added by @AlexWaygood on 2025-11-15 19:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-15 19:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-15 19:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-15 19:44_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-15 19:44_

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 19:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-15 19:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
stone (https://github.com/dropbox/stone)
- stone/backends/python_rsrc/stone_validators.py:714:16: error[invalid-exception-caught] Cannot catch object of type `list[Unknown | <class 'AttributeError'> | <class 'ValueError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ stone/backends/python_rsrc/stone_validators.py:714:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `list[Unknown | <class 'AttributeError'> | <class 'ValueError'>]`

beartype (https://github.com/beartype/beartype)
- beartype/typing/_typingcache.py:130:23: error[invalid-raise] Cannot raise object of type `object` (must be a `BaseException` subclass or instance)
+ beartype/typing/_typingcache.py:130:23: error[invalid-raise] Cannot raise object of type `object`: Not an instance or subclass of `BaseException`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/utils/_indexing.py:348:15: error[invalid-raise] Cannot raise object of type `None` (must be a `BaseException` subclass or instance)
+ sklearn/utils/_indexing.py:348:15: error[invalid-raise] Cannot raise object of type `None`: Not an instance or subclass of `BaseException`

manticore (https://github.com/trailofbits/manticore)
- manticore/platforms/linux.py:328:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:328:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?
- manticore/platforms/linux.py:331:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:331:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?
- manticore/platforms/linux.py:334:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:334:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?
- manticore/platforms/linux.py:341:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:341:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?
- manticore/platforms/linux.py:344:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:344:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?
- manticore/platforms/linux.py:347:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:347:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?
- manticore/platforms/linux.py:350:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:350:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?
- manticore/platforms/linux.py:353:15: error[invalid-raise] Cannot raise object of type `NotImplementedType` (must be a `BaseException` subclass or instance)
+ manticore/platforms/linux.py:353:15: error[invalid-raise] Cannot raise `NotImplemented`: Did you mean `NotImplementedError`?

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/dbg/lldb/__init__.py:527:23: error[invalid-raise] Cannot raise object of type `None | Error` (must be a `BaseException` subclass or instance)
+ pwndbg/dbg/lldb/__init__.py:527:23: error[invalid-raise] Cannot raise object of type `None | Error`: Not an instance or subclass of `BaseException`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/flow_engine.py:363:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)` (must be a `BaseException` subclass or instance)
+ src/prefect/flow_engine.py:363:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)`: Not an instance or subclass of `BaseException`
- src/prefect/flow_engine.py:950:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)` (must be a `BaseException` subclass or instance)
+ src/prefect/flow_engine.py:950:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)`: Not an instance or subclass of `BaseException`
- src/prefect/states.py:649:11: error[invalid-raise] Cannot raise object of type `Unknown | Coroutine[Any, Any, Unknown]` (must be a `BaseException` subclass or instance)
+ src/prefect/states.py:649:11: error[invalid-raise] Cannot raise object of type `Unknown | Coroutine[Any, Any, Unknown]`: Not an instance or subclass of `BaseException`
- src/prefect/task_engine.py:499:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)` (must be a `BaseException` subclass or instance)
+ src/prefect/task_engine.py:499:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)`: Not an instance or subclass of `BaseException`
- src/prefect/task_engine.py:1107:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)` (must be a `BaseException` subclass or instance)
+ src/prefect/task_engine.py:1107:23: error[invalid-raise] Cannot raise object of type `BaseException | (type[NotSet] & ~<class 'NotSet'>) | (Unknown & ~<class 'NotSet'>)`: Not an instance or subclass of `BaseException`

pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/test/testvb.py:251:12: error[invalid-exception-caught] Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ com/win32com/test/testvb.py:251:12: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `Unknown | None`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/vendor/psutil/_psbsd.py:509:12: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- ddtrace/vendor/psutil/_psbsd.py:511:12: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ddtrace/vendor/psutil/_psbsd.py:509:12: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
+ ddtrace/vendor/psutil/_psbsd.py:511:12: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
- ddtrace/vendor/psutil/_psosx.py:252:16: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- ddtrace/vendor/psutil/_psosx.py:321:16: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- ddtrace/vendor/psutil/_psosx.py:323:16: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ddtrace/vendor/psutil/_psosx.py:252:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
+ ddtrace/vendor/psutil/_psosx.py:321:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
+ ddtrace/vendor/psutil/_psosx.py:323:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
- ddtrace/vendor/psutil/_psosx.py:363:20: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ddtrace/vendor/psutil/_psosx.py:363:20: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
- ddtrace/vendor/psutil/_pssunos.py:472:16: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- ddtrace/vendor/psutil/_pssunos.py:482:16: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ddtrace/vendor/psutil/_pssunos.py:472:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
+ ddtrace/vendor/psutil/_pssunos.py:482:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`
- ddtrace/vendor/psutil/_pswindows.py:783:20: error[invalid-exception-caught] Cannot catch object of type `None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ddtrace/vendor/psutil/_pswindows.py:783:20: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `None`

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/druid/__init__.py:174:16: error[invalid-exception-caught] Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ibis/backends/druid/__init__.py:174:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `Unknown | None`
- ibis/backends/flink/__init__.py:325:16: error[invalid-exception-caught] Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ibis/backends/flink/__init__.py:325:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `Unknown | None`
- ibis/backends/flink/__init__.py:1051:16: error[invalid-exception-caught] Cannot catch object of type `Unknown | None` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ ibis/backends/flink/__init__.py:1051:16: error[invalid-exception-caught] Invalid object caught in an exception handler: Object has type `Unknown | None`

sympy (https://github.com/sympy/sympy)
- sympy/utilities/codegen.py:959:23: error[invalid-raise] Cannot raise object of type `CodeGen` (must be a `BaseException` subclass or instance)
+ sympy/utilities/codegen.py:959:23: error[invalid-raise] Cannot raise object of type `CodeGen`: Not an instance or subclass of `BaseException`


```

</details>


No memory usage changes detected ✅



---

_@carljm approved on 2025-11-15 21:57_

Nice

---

_Merged by @AlexWaygood on 2025-11-15 22:12_

---

_Closed by @AlexWaygood on 2025-11-15 22:12_

---

_Branch deleted on 2025-11-15 22:12_

---
