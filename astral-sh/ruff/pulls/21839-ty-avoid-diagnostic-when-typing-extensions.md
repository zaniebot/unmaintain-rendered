```yaml
number: 21839
title: "[ty] Avoid diagnostic when `typing_extensions.ParamSpec` uses `default` parameter"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/typing-extensions-paramspec
created_at: 2025-12-08T06:15:07Z
updated_at: 2025-12-08T12:34:31Z
url: https://github.com/astral-sh/ruff/pull/21839
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Avoid diagnostic when `typing_extensions.ParamSpec` uses `default` parameter

---

_@dhruvmanila_

## Summary

fixes: https://github.com/astral-sh/ty/issues/1798

## Test Plan

Add mdtest.


---

_Label `bug` added by @dhruvmanila on 2025-12-08 06:15_

---

_Review requested from @carljm by @dhruvmanila on 2025-12-08 06:15_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-12-08 06:15_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-08 06:15_

---

_Label `ty` added by @dhruvmanila on 2025-12-08 06:15_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-08 06:15_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 06:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 06:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Comment by @dhruvmanila on 2025-12-08 06:49_

I need to look at the new fuzzer panics (wasn't expecting them), will do it in a bit (lunch time).

---

_Comment by @dhruvmanila on 2025-12-08 11:22_

I'm not really sure why is the fuzzer panic showing up in CI because I tried running those code snippets locally and they all are working fine (no panics).

---

_Comment by @dhruvmanila on 2025-12-08 11:29_

Running the fuzzer locally, I'm seeing at least the following panic randomly on seed 13:

```
ty stderr output:
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:3212:44 when checking `/var/folders/0h/7pwtlzd924l7ydmq5kwvd2dc0000gn/T/tmpmdthud5d/input.py`: ``Type::to_instance()` should always return `Some()` if called on a type assignable to `type[BaseException]``
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.14.8+39 (12472ab8a 2025-12-08)
info: Args: ["/Users/dhruv/work/astral/ruff/target/profiling/ty", "check", "/var/folders/0h/7pwtlzd924l7ydmq5kwvd2dc0000gn/T/tmpmdthud5d/input.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1002))
             at crates/ty_python_semantic/src/types/infer.rs:69
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535
```

The code is:
```py
class name_3[**name_5]:
    (name_3 := name_5)
    try:
        pass
    except name_3:
        pass
```

which seems unrelated to the changes made in this PR?

---

_Comment by @AlexWaygood on 2025-12-08 11:34_

> which seems unrelated to the changes made in this PR?

Do we infer the `name_5` ParamSpec as an instance of `typing.ParamSpec` or `typing_extensions.ParamSpec`? Is the answer to that question the same with `--python-version=3.9` as it is with `--python-version=3.10`? When you run ty on these code snippets locally, are you passing a `--python-version` argument at all — if you do, does that change things?

---

_Comment by @dhruvmanila on 2025-12-08 11:40_

> Do we infer the `name_5` ParamSpec as an instance of `typing.ParamSpec` or `typing_extensions.ParamSpec`? Is the answer to that question the same with `--python-version=3.9` as it is with `--python-version=3.10`?

Ohh, thanks! I can reproduce the panic reliably now. It panics on 3.9, but not on 3.10 which is because ParamSpec was added in 3.10, so I'd need to change it somewhere to account for that.

---

_Comment by @AlexWaygood on 2025-12-08 11:42_

I'll make some updates to the fuzzer output so that it prints the Python version that it used to test the snippet 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:81 on 2025-12-08 12:23_

is it worth adding a bit more to this test, to demonstrate that `P1` is understood by ty in exactly the same way as a `typing.ParamSpec` with the `default=` argument on Python 3.13+?

---

_@AlexWaygood approved on 2025-12-08 12:23_

Thank you!

---

_@dhruvmanila reviewed on 2025-12-08 12:27_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:81 on 2025-12-08 12:27_

Yeah, I think that makes sense. I'll add them.

---

_Comment by @dhruvmanila on 2025-12-08 12:30_

> Thank you!

ty for pointing me in the right direction about the fuzzer panic!

---

_Merged by @dhruvmanila on 2025-12-08 12:34_

---

_Closed by @dhruvmanila on 2025-12-08 12:34_

---

_Branch deleted on 2025-12-08 12:34_

---
