```yaml
number: 21937
title: "[ty] Use datatest instead of dirtest"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/ty-datatest
created_at: 2025-12-12T08:40:45Z
updated_at: 2025-12-18T18:05:03Z
url: https://github.com/astral-sh/ruff/pull/21937
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Use datatest instead of dirtest

---

_Pull request opened by @MichaReiser on 2025-12-12 08:40_

## Summary

Same as https://github.com/astral-sh/ruff/pull/21933 but for our mdtests.

[`datatest-stable`](https://github.com/nextest-rs/datatest-stable) uses a custom test-harness that mimics `cargo test` and supports `cargo nextest` to create a separate test for every file found in a given directory.
Unlike `dir-tests`, `datatest-stable` doesn't require re-compilation after adding or removing a test file (because it uses a custom test harness). This should allow us (people like me who aren't using the mdtest Python script) to iterate faster on mdtests, since we no longer have to recompile `ty_python_semantic` whenever a markdown file changes.

## Test plan

`mdtests` on main:

```
     Summary [  10.810s] 301 tests run: 301 passed, 0 skipped
```

`mdtests` on this branch:

```
Summary [  10.606s] 301 tests run: 301 passed, 0 skipped
```

I used the `mdtest.py` file (for the first time) and verified that it picks up changes and only runs the changed tests. 

I verified that a new test file is picked up without needing to recompile `ty_python_semantic`.

---

_Label `testing` added by @MichaReiser on 2025-12-12 08:40_

---

_Label `ty` added by @MichaReiser on 2025-12-12 08:40_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2794 diagnostics
+ Found 2797 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
- tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`

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

_Comment by @astral-sh-bot[bot] on 2025-12-12 08:54_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2025-12-12 08:54_

---

_Review requested from @carljm by @MichaReiser on 2025-12-12 08:54_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-12 08:54_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-12 08:54_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-12 08:54_

---

_@AlexWaygood reviewed on 2025-12-12 09:01_

Nice! I think this means that we can get rid of the "note" block here saying that you probably need a build.rs file if you use mdtests in a crate: https://github.com/astral-sh/ruff/blob/0138cd238a8669d53c325eb7ca19155946dfa665/crates/ty_test/README.md?plain=1#L37

We should instead just say that it's recommended to use mdtest in combination with datatest.

Also, you should use mdtest.py 😆 the experience is soooo much better!!

---

_Comment by @MichaReiser on 2025-12-12 09:27_

> Also, you should use mdtest.py 😆 the experience is soooo much better!!

I believe you. I just don't write many mdtests (I only occasionally debug them)

---

_Comment by @dhruvmanila on 2025-12-12 13:03_

Uff, I've lost the ability to select individual tests from the editor :(

i.e., rust-analyzer cannot pick up individual tests in this file now and is the same across other tests as well (parser and formatter)

<img width="2560" height="1384" alt="Screenshot 2025-12-12 at 18 31 31" src="https://github.com/user-attachments/assets/d6f160a5-e924-451a-bff8-ba5d246d9293" />


---

_Comment by @dhruvmanila on 2025-12-12 13:07_

For context, on the right split you can see that rust-analyzer is able to provide me with all the individual tests aka the file name in mdtest suite and I can select on any one of them and it will only run that specific test and then re-run that same test however many times I want:


https://github.com/user-attachments/assets/5b54208c-50a3-49d3-b129-a0c6472e7dbe



---

_Comment by @MichaReiser on 2025-12-12 13:08_

Hmm. I guess that's not unexpected given the approach (note, I don't think this worked for the formatter or parser before because we only had one test). Before, rust-analyzer was able to see all tests because of the macro expansion (but I doubt that it ever updated when files were added or removed). Now, there's no such macro expansion happening anymore. 



---

_Comment by @dhruvmanila on 2025-12-12 13:10_

> Before, rust-analyzer was able to see all tests because of the macro expansion (but I doubt that it ever updated when files were added or removed).

Yeah, in those cases I'd ask rust-analyzer to [re-build the proc macros](https://rust-analyzer.github.io/book/contributing/lsp-extensions.html#rebuild-proc-macros) which would then update this list. Note that this is different than restarting the server.

---

_Comment by @dhruvmanila on 2025-12-12 13:14_

I'm not against this change, it's just that this has been a core part of my workflow. I guess I'll have to try out `mdtest.py` finally :)

---

_Comment by @MichaReiser on 2025-12-12 13:30_

> I'm not against this change, it's just that this has been a core part of my workflow. I guess I'll have to try out `mdtest.py` finally :)

Yeah. Losing that certainly feels painful. We might need to write a mdtest LSP

---

_Comment by @MichaReiser on 2025-12-15 08:20_

I spent some time trying to fix the LSP regression but, unfortunately, I think this will require changes in r-a. I hoped that all that was missing is `--list --output=json` support in libtest-mimic (which indeed is missing), but r-a only uses semantic analysis to find tests (for good reasons) and I haven't found a way to customize r-a's test discovery. 

I opened https://github.com/rust-lang/rust-analyzer/issues/21259 upstream

---

_Comment by @dhruvmanila on 2025-12-15 08:37_

Wow, thank you for spending time in looking into this, I appreciate that!

---

_@dhruvmanila approved on 2025-12-15 08:38_

---

_Merged by @MichaReiser on 2025-12-18 18:05_

---

_Closed by @MichaReiser on 2025-12-18 18:05_

---

_Branch deleted on 2025-12-18 18:05_

---
