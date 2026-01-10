```yaml
number: 21923
title: "[ty] Squash false positive logs for failing to find `builtins` as a real module"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/squash-false-positive-log
created_at: 2025-12-11T16:34:23Z
updated_at: 2025-12-11T17:50:10Z
url: https://github.com/astral-sh/ruff/pull/21923
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Squash false positive logs for failing to find `builtins` as a real module

---

_Pull request opened by @BurntSushi on 2025-12-11 16:34_

I recently started noticing this showing up in the logs for every scope
based completion request:

```
2025-12-11 11:25:35.704329935 DEBUG request{id=29 method="textDocument/completion"}:map_stub_definition: Module `builtins` not found while looking in parent dirs
```

And in particular, it was repeated several times. This was confusing to
me because, well, of course `builtins` should resolve.

This particular code path comes from looking for the docstrings
of completion items. This involves a spelunking that ultimately
tries to resolve a "real" module if the stub doesn't have available
docstrings. But I guess there is no "real" `builtins` module, so
`resolve_real_module` fails. Which is fine, but the noisy logs were
annoying since this is an expected case.

So here, we carve out a short circuit for `builtins` and also improve
the log message.


---

_Review requested from @carljm by @BurntSushi on 2025-12-11 16:34_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-11 16:34_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-11 16:34_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-11 16:34_

---

_Label `server` added by @BurntSushi on 2025-12-11 16:34_

---

_Label `ty` added by @BurntSushi on 2025-12-11 16:34_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-11 16:34_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-11 16:34_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-11 16:34_

---

_Review requested from @Gankra by @BurntSushi on 2025-12-11 16:34_

---

_Label `internal` added by @BurntSushi on 2025-12-11 16:34_

---

_Comment by @AlexWaygood on 2025-12-11 16:36_

> But I guess there is no "real" `builtins` module, so
> `resolve_real_module` fails.

Well, there _is_, but it's written in C, so we don't know anything about it 😛

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 16:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 16:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_@Gankra approved on 2025-12-11 16:53_

---

_Comment by @AlexWaygood on 2025-12-11 16:55_

Rather than hardcoding a very specific exception for `builtins`, you could skip the spelunk for any module where this function returns `true`: https://github.com/astral-sh/ruff/blob/5a9d6a91ea836fbaf14c9be3e8bb01a238f1158d/crates/ruff_python_stdlib/src/sys/builtin_modules.rs#L13

We know those modules are all written in C, so attempting to resolve the "real module" is guaranteed to fail for all of them.

---

_@AlexWaygood approved on 2025-12-11 17:16_

---

_Merged by @BurntSushi on 2025-12-11 17:50_

---

_Closed by @BurntSushi on 2025-12-11 17:50_

---

_Branch deleted on 2025-12-11 17:50_

---
