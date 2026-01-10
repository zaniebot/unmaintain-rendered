```yaml
number: 21852
title: "[ty] Teach `ty check` to ask uv to sync the venv of a PEP-723 script"
type: pull_request
state: open
author: Gankra
labels:
  - configuration
  - ty
assignees: []
draft: true
base: main
head: gankra/script
created_at: 2025-12-08T20:09:04Z
updated_at: 2025-12-09T16:05:54Z
url: https://github.com/astral-sh/ruff/pull/21852
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Teach `ty check` to ask uv to sync the venv of a PEP-723 script

---

_Pull request opened by @Gankra on 2025-12-08 20:09_

## Summary

If the argument to `ty check` is a single path, and that path contains a PEP-723 script:

* we will parse the metadata to retrieve the `requires-python` and the `tool.ty`
* we will invoke `uv sync --output-format=json --script` to ask it to create a temp python venv for the script's dependencies, and use that as the venv instead of the ambient workspace venv

If the `uv sync` component fails for any reason we will silently fallback to the ambient workspace venv.

I haven't really thought too hard about the implications/design/testing here, right now this is a proof-of-concept.

Part of https://github.com/astral-sh/ty/issues/691

## Test Plan

With this change `ty check crates/ty_python_semantic/mdtest.py` passes


---

_Label `configuration` added by @Gankra on 2025-12-08 20:09_

---

_Label `ty` added by @Gankra on 2025-12-08 20:09_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 20:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 20:13_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5544 diagnostics
+ Found 5545 diagnostics

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

_Comment by @Gankra on 2025-12-08 20:15_

Vague future work thoughts:

In the LSP we can potentially use this machinery to:
* Detect we're in a PEP script
* Check if the PEP script env diverges meaningfully from the workspace
  * Does the requires_python mismatch?
  * Does it have any dependencies?
* If there's notable divergences we can disable checking or emit a warning or something
 
More broadly these scripts should ideally just spin up their own workspaces but we're not good at multi-workspaces.

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 20:18_


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

_Comment by @Gankra on 2025-12-08 20:31_

Core questions to answer: 

* When is this safe to do?
* Can we pass extra flags to `uv` (like `--no-build`) to security-harden? (What would break?)
* Must the user opt into this?
* In the LSP can we use vscode's "do you trust this workspace" prompt to enable this?

---

_Comment by @MichaReiser on 2025-12-08 21:15_

I haven't looked at the implementation but here a few more open questions:

* Is there any configuration inheritance between the project and script, if so, in what direction (does it include `overrides`)
* How are scripts checked when checking the entire project (I think it's fine to check them with the project settings for now, users can exclude them if they don't want that)


> More broadly these scripts should ideally just spin up their own workspaces but we're not good at multi-workspaces.

We can spin up its own database. The only thing that's awkward about it in an LSP use case (this also is a problem for syncing) is that a regular Python file can become a script after every keystroke (or the other way round), and dependencies can change as well. This might be an action we only want to perform on save? 

---

_Comment by @Gankra on 2025-12-09 16:02_

* Is invoking `Command::new("uv")` appropriate here? Should we be "getting" uv some other way?
* I suppose when there's a uv vscode extension we would want to query that for what `uv` to use in the ty extension?
  * So we shouldn't consider bundling a copy of `uv` in ty's vscode extension

---
