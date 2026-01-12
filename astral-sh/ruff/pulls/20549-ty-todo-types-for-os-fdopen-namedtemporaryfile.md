```yaml
number: 20549
title: "[ty] Todo-types for `os.fdopen`, `NamedTemporaryFile`, and `Path.open`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/open-calls
created_at: 2025-09-24T12:43:46Z
updated_at: 2025-09-24T13:44:00Z
url: https://github.com/astral-sh/ruff/pull/20549
synced_at: 2026-01-12T15:57:04Z
```

# [ty] Todo-types for `os.fdopen`, `NamedTemporaryFile`, and `Path.open`

---

_@sharkdp_

## Summary

This applies the trick that we use for `builtins.open` to similar functions that have the same problem. The reason is that the problem would otherwise become even more pronounced once we add understanding of the implicit type of `self` parameters, because then something like `(base_path / "test.bin").open("rb")` also leads to a wrong return type and can result in false positives.

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-09-24 12:43_

---

_Comment by @github-actions[bot] on 2025-09-24 12:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-24 12:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
jinja (https://github.com/pallets/jinja)
- src/jinja2/bccache.py:298:39: error[invalid-argument-type] Argument to bound method `write_bytecode` is incorrect: Expected `IO[bytes]`, found `_TemporaryFileWrapper[str]`
- Found 185 diagnostics
+ Found 184 diagnostics

isort (https://github.com/pycqa/isort)
- isort/api.py:161:5: error[invalid-assignment] Object of type `(str & ~AlwaysFalsy) | (Path & ~AlwaysTruthy & ~AlwaysFalsy)` is not assignable to `str | None`
- isort/api.py:214:13: error[invalid-argument-type] Argument to function `process` is incorrect: Expected `str`, found `str | None`
- Found 33 diagnostics
+ Found 31 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/extensions/feedexport.py:200:16: error[invalid-return-type] Return type does not match returned value: expected `IO[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
- Found 1045 diagnostics
+ Found 1044 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/contrib/swift.py:885:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
- dulwich/object_store.py:1522:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[bytes] | None`, found `TextIOWrapper[_WrappedBuffer]`
- Found 179 diagnostics
+ Found 177 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- src/check_jsonschema/cachedownloader.py:82:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- Found 56 diagnostics
+ Found 55 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/utils.py:109:12: error[invalid-return-type] Return type does not match returned value: expected `BufferedWriter`, found `TextIOWrapper[_WrappedBuffer]`
- Found 172 diagnostics
+ Found 171 diagnostics

manticore (https://github.com/trailofbits/manticore)
- tests/native/test_lazy_memory.py:127:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_lazy_memory.py:140:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_lazy_memory.py:152:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_lazy_memory.py:164:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_lazy_memory.py:176:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:938:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:951:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:963:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:975:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:987:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1005:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1031:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1039:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1053:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1527:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1557:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1570:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1594:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1652:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- tests/native/test_memory.py:1706:9: error[no-matching-overload] No overload of bound method `write` matches arguments
- Found 1097 diagnostics
+ Found 1077 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/model_selection/tests/test_validation.py:2028:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- Found 1955 diagnostics
+ Found 1954 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
- Lib/test/test_tempfile.py:986:13: error[no-matching-overload] No overload of bound method `write` matches arguments
- Lib/test/test_urllib.py:640:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `str`, found `Unknown | Literal[b""]`
- Found 23902 diagnostics
+ Found 23900 diagnostics

CPython (peg_generator) (https://github.com/python/cpython)
- Lib/test/test_tempfile.py:986:13: error[no-matching-overload] No overload of bound method `write` matches arguments
- Lib/test/test_urllib.py:640:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `str`, found `Unknown | Literal[b""]`
- Found 23901 diagnostics
+ Found 23899 diagnostics

CPython (Argument Clinic) (https://github.com/python/cpython)
- Lib/test/test_tempfile.py:986:13: error[no-matching-overload] No overload of bound method `write` matches arguments
- Lib/test/test_urllib.py:640:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `str`, found `Unknown | Literal[b""]`
- Found 23902 diagnostics
+ Found 23900 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-09-24 12:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9702 on 2025-09-24 12:46_

This is overly aggressive, because now we replace the whole signature with `(*args, **kwargs) -> @Todo`, i.e. we also have false negatives for wrong `Path.open` calls, and return a `@Todo` type for every call to `Path.open`. But manually constructing a overload set for `Path.open` seems daunting.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-24 12:50_

---

_Marked ready for review by @sharkdp on 2025-09-24 12:51_

---

_Review requested from @carljm by @sharkdp on 2025-09-24 12:51_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-24 12:51_

---

_Review requested from @dcreager by @sharkdp on 2025-09-24 12:51_

---

_Comment by @github-actions[bot] on 2025-09-24 12:55_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `no-matching-overload` | 0 | 22 | 0 |
| `invalid-argument-type` | 0 | 4 | 0 |
| `invalid-return-type` | 0 | 2 | 0 |
| `invalid-assignment` | 0 | 1 | 0 |
| **Total** | **0** | **29** | **0** |

**[Full report with detailed diff](https://david-open-calls.ecosystem-663.pages.dev/diff)**


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1754 on 2025-09-24 13:05_

could maybe combine these two branches


```suggestion
            KnownFunction::Open | KnownFunction::Fdopen => {
                // TODO: Temporary special-casing for `builtins.open`/`os.fdopen` to avoid an excessive number of
                // false positives in lieu of proper support for PEP-613 type aliases.
                if let [_, Some(mode), ..] = parameter_types
                    && is_mode_with_nontrivial_return_type(db, *mode)
                {
                    overload.set_return_type(todo_type!("`builtins.open`/`os.fdopen` return type"));
                }
            }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1695 on 2025-09-24 13:05_

I guess I originally introduced this typo in my previous PR 🙈

```suggestion
                // false positives in lieu of proper support for PEP-613 type aliases.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:391 on 2025-09-24 13:08_

The runtime type of `Path("foo").open` is a `types.MethodType` instance rather than a `types.MethodWrapperType` instance, since `pathlib.Path` is just a pure-Python class with regular pure-Python methods

```suggestion
                f.write_str("bound method `Path.open`")
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9605 on 2025-09-24 13:10_

```pycon
>>> import pathlib
>>> pathlib.Path("foo").open
<bound method Path.open of PosixPath('foo')>
>>> import types
>>> type(_) is types.MethodType
True
```

```suggestion
            KnownBoundMethodType::PathOpen => KnownClass::MethodType,
```

---

_@AlexWaygood approved on 2025-09-24 13:10_

---

_@AlexWaygood reviewed on 2025-09-24 13:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/open_calls.rs`:6 on 2025-09-24 13:18_

this helper is only used from `function.rs`, and it might be easier to rip out the short-term hack out again later if this helper lived in `function.rs` rather than introducing a new module? Don't have a strong opinion though

---

_@sharkdp reviewed on 2025-09-24 13:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/open_calls.rs`:6 on 2025-09-24 13:21_

Right. I wanted to use it for `Path.open`, but then decided otherwise. Will move it back.

---

_@sharkdp reviewed on 2025-09-24 13:23_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9605 on 2025-09-24 13:23_

Oops. I misinterpreted the docstring.

---

_@sharkdp reviewed on 2025-09-24 13:32_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:1754 on 2025-09-24 13:32_

Will leave it as-is. We'll hopefully remove it soon.

---

_Merged by @sharkdp on 2025-09-24 13:43_

---

_Closed by @sharkdp on 2025-09-24 13:43_

---

_Branch deleted on 2025-09-24 13:44_

---
