```yaml
number: 18245
title: "[ty] Split `invalid-base` error code into two error codes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/split-invalid-base
created_at: 2025-05-21T20:13:42Z
updated_at: 2025-05-21T22:02:41Z
url: https://github.com/astral-sh/ruff/pull/18245
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Split `invalid-base` error code into two error codes

---

_Pull request opened by @AlexWaygood on 2025-05-21 20:13_

## Summary

Our `invalid-base` diagnostic currently catches two somewhat distinct things:
1. It catches class bases that will actually cause the class definition to raise an exception at runtime (any class base where the type of the base is not assignable to `type`)
2. It catches class bases that will probably not raise an exception at runtime, but which we cannot allow to pass without a diagnostic because we would not be able to accurately infer the MRO of the resulting class (union types, `type[]` types, instances of `type`, etc.)

The fact that it's doing two distinct things makes it hard to document the lint, and would also make it hard for users to suppress the lint in a fine-grained way (the first category of errors is much more serious than the second category). This PR therefore splits the diagnostic into two: `invalid-base` and `unsupported-base`. It also adds documentation for both new error codes.

## Test Plan

Snapshots


---

_Label `ty` added by @AlexWaygood on 2025-05-21 20:13_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-21 20:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:911 on 2025-05-21 20:14_

I moved this check into `ClassBase::try_from_ty` so that we don't create the `MroError` in the first place for classes that inherit from `Never`

---

_Comment by @github-actions[bot] on 2025-05-21 20:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pybind11 (https://github.com/pybind/pybind11)
- error[invalid-base] pybind11/setup_helpers.py:89:25: Invalid class base with type `<class 'Extension'> | <class 'Extension'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] pybind11/setup_helpers.py:89:25: Unsupported class base with type `<class 'Extension'> | <class 'Extension'>`
- error[invalid-base] pybind11/setup_helpers.py:271:17: Invalid class base with type `<class 'build_ext'> | <class 'build_ext'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] pybind11/setup_helpers.py:271:17: Unsupported class base with type `<class 'build_ext'> | <class 'build_ext'>`

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-base] src/werkzeug/serving.py:879:25: Invalid class base with type `<class 'ForkingMixIn'> | <class 'ForkingMixIn'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] src/werkzeug/serving.py:879:25: Unsupported class base with type `<class 'ForkingMixIn'> | <class 'ForkingMixIn'>`

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-base] tests/test_utils_deprecate.py:41:29: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:65:29: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:84:29: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:101:29: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:104:28: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:107:28: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:123:29: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] tests/test_utils_deprecate.py:41:29: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:65:29: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:84:29: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:101:29: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:104:28: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:107:28: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:123:29: Unsupported class base with type `type`
- error[invalid-base] tests/test_utils_deprecate.py:142:30: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:160:38: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:163:39: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] tests/test_utils_deprecate.py:142:30: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:160:38: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:163:39: Unsupported class base with type `type`
- error[invalid-base] tests/test_utils_deprecate.py:197:38: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:200:39: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] tests/test_utils_deprecate.py:197:38: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:200:39: Unsupported class base with type `type`
- error[invalid-base] tests/test_utils_deprecate.py:251:29: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[invalid-base] tests/test_utils_deprecate.py:268:28: Invalid class base with type `type` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] tests/test_utils_deprecate.py:251:29: Unsupported class base with type `type`
+ warning[unsupported-base] tests/test_utils_deprecate.py:268:28: Unsupported class base with type `type`

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-base] tests/test_copy.py:303:23: Invalid class base with type `<class 'StrDumper'> | <class 'StrBinaryDumper'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] tests/test_copy.py:303:23: Unsupported class base with type `<class 'StrDumper'> | <class 'StrBinaryDumper'>`
- error[invalid-base] tests/test_copy_async.py:313:23: Invalid class base with type `<class 'StrDumper'> | <class 'StrBinaryDumper'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] tests/test_copy_async.py:313:23: Unsupported class base with type `<class 'StrDumper'> | <class 'StrBinaryDumper'>`

meson (https://github.com/mesonbuild/meson)
- error[invalid-base] mesonbuild/_pathlib.py:42:16: Invalid class base with type `type[Path]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] mesonbuild/_pathlib.py:42:16: Unsupported class base with type `type[Path]`

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-base] pwndbg/aglib/heap/structs.py:65:15: Invalid class base with type `<class 'c_uint32'> | <class 'c_uint64'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] pwndbg/aglib/heap/structs.py:65:15: Unsupported class base with type `<class 'c_uint32'> | <class 'c_uint64'>`
- error[invalid-base] pwndbg/aglib/heap/structs.py:71:16: Invalid class base with type `<class 'c_uint32'> | <class 'c_uint64'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] pwndbg/aglib/heap/structs.py:71:16: Unsupported class base with type `<class 'c_uint32'> | <class 'c_uint64'>`

mypy (https://github.com/python/mypy)
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:403:19: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:403:19: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:408:21: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:408:21: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:413:23: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:413:23: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:418:21: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:418:21: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:423:21: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:423:21: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:428:19: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:428:19: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:433:21: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:433:21: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:442:13: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:442:13: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:447:16: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:447:16: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:455:16: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:455:16: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:460:33: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:460:33: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:466:35: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:466:35: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:475:39: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:475:39: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:501:70: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:501:70: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:504:80: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:504:80: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:507:17: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:507:17: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:515:44: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:515:44: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:538:5: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:538:5: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:543:21: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:543:21: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:548:43: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:548:43: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:554:49: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:554:49: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:571:17: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:571:17: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:577:53: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:577:53: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:660:66: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:660:66: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:691:32: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:691:32: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing.pyi:764:10: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing.pyi:764:10: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:416:23: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:416:23: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:421:25: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:421:25: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:426:27: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:426:27: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:431:25: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:431:25: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:436:25: Invalid class base with type `_SpecialForm` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:436:25: Invalid class base with type `_SpecialForm`
- error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:441:23: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:441:23: Invalid class base with type `object`
- error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:446:25: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypy/typeshed/stdlib/typing_extensions.pyi:446:25: Invalid class base with type `object`
- error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:54:13: Invalid class base with type `Literal[0]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:54:13: Invalid class base with type `Literal[0]`
- error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:64:32: Invalid class base with type `Literal[0]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:64:32: Invalid class base with type `Literal[0]`
- error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:121:39: Invalid class base with type `Literal[0]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:121:39: Invalid class base with type `Literal[0]`
- error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:145:19: Invalid class base with type `Literal[0]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:145:19: Invalid class base with type `Literal[0]`
- error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:148:21: Invalid class base with type `Literal[0]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] mypyc/test-data/fixtures/typing-full.pyi:148:21: Invalid class base with type `Literal[0]`

cloud-init (https://github.com/canonical/cloud-init)
- error[invalid-base] cloudinit/templater.py:89:30: Invalid class base with type `<class 'DebugUndefined'> | <class 'object'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] cloudinit/templater.py:89:30: Unsupported class base with type `<class 'DebugUndefined'> | <class 'object'>`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-base] ddtrace/appsec/_iast/_taint_tracking/_vendor/pybind11/pybind11/setup_helpers.py:89:25: Invalid class base with type `<class 'Extension'> | <class 'Extension'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] ddtrace/appsec/_iast/_taint_tracking/_vendor/pybind11/pybind11/setup_helpers.py:89:25: Unsupported class base with type `<class 'Extension'> | <class 'Extension'>`
- error[invalid-base] ddtrace/appsec/_iast/_taint_tracking/_vendor/pybind11/pybind11/setup_helpers.py:271:17: Invalid class base with type `<class 'build_ext'> | <class 'build_ext'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] ddtrace/appsec/_iast/_taint_tracking/_vendor/pybind11/pybind11/setup_helpers.py:271:17: Unsupported class base with type `<class 'build_ext'> | <class 'build_ext'>`

scipy (https://github.com/scipy/scipy)
- error[invalid-base] scipy/stats/_distribution_infrastructure.py:4139:30: Invalid class base with type `<class 'ContinuousDistribution'> | <class 'DiscreteDistribution'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[unsupported-base] scipy/stats/_distribution_infrastructure.py:4139:30: Unsupported class base with type `<class 'ContinuousDistribution'> | <class 'DiscreteDistribution'>`

```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/mro.rs`:1 on 2025-05-21 20:18_

The changes in this file are a drive-by fix to try to solve the indeterminacy we've seen in the order of some of our `duplicate-base` diagnostics. See e.g. https://github.com/astral-sh/ruff/pull/18226#discussion_r2098690793 -- it's been changing randomly on several PRs that touch `mro.rs`

---

_@AlexWaygood reviewed on 2025-05-21 20:19_

---

_Marked ready for review by @AlexWaygood on 2025-05-21 20:19_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-21 20:19_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-21 20:19_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-21 20:19_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-21 20:19_

---

_Renamed from "[ty] Split `invalid-base` diagnostic into two diagnostics" to "[ty] Split `invalid-base` error code into two error codes" by @AlexWaygood on 2025-05-21 20:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:501 on 2025-05-21 20:47_

Arguably this should be a warning instead of an error?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:2031 on 2025-05-21 20:49_

We could also issue `unsupported-base` for types with an `__mro_entries__` member?

---

_@carljm approved on 2025-05-21 20:50_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-21 21:51_

---

_@carljm approved on 2025-05-21 22:00_

Nice!

---

_Merged by @AlexWaygood on 2025-05-21 22:02_

---

_Closed by @AlexWaygood on 2025-05-21 22:02_

---

_Branch deleted on 2025-05-21 22:02_

---
