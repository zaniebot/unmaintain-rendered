```yaml
number: 21970
title: "[ty] Include `=` in keyword-argument autocomplete suggestion labels"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - server
  - ty
assignees: []
base: main
head: alex/keyword-completion-labels
created_at: 2025-12-13T23:11:52Z
updated_at: 2025-12-15T15:51:07Z
url: https://github.com/astral-sh/ruff/pull/21970
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Include `=` in keyword-argument autocomplete suggestion labels

---

_@AlexWaygood_

## Summary

It wasn't obvious at all when editing some Python code just now in VSCode that the `frozen` autocomplete suggestion here was actually a keyword-argument autocomplete suggestion that would insert `frozen=` into my function call:

<img width="1374" height="642" alt="image" src="https://github.com/user-attachments/assets/c857b57a-13be-43a9-aa1f-e451b0a5206b" />

In fact, I was _looking_ for that autocomplete suggestion, but I assumed the `frozen` option was just going to insert `frozen` into my call; I assumed that `frozen` referred to some other variable in my code somewhere!

This PR adjusts the autocomplete labels for keyword-argument completions so that they include the `=` in the label. With this PR, that list looks like this, which makes it much easier to understand what accepting the `frozen` suggestion is going to do to my code:

<img width="1332" height="508" alt="image" src="https://github.com/user-attachments/assets/6add8d11-6ae4-41e4-969b-f95197e75536" />

This also matches how Pylance renders the labels for keyword-argument completions:

<img width="1444" height="288" alt="image" src="https://github.com/user-attachments/assets/b0355f41-d55c-403d-ab8c-bcb829475cef" />

## Test Plan

Snapshots updated. Plus, see screenshots above.


---

_Review requested from @BurntSushi by @AlexWaygood on 2025-12-13 23:11_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-13 23:11_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-13 23:11_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-13 23:11_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-13 23:11_

---

_Label `server` added by @AlexWaygood on 2025-12-13 23:11_

---

_Label `ty` added by @AlexWaygood on 2025-12-13 23:11_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 23:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-13 23:16_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5136 diagnostics
+ Found 5137 diagnostics

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

_@AlexWaygood reviewed on 2025-12-13 23:19_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:2427 on 2025-12-13 23:19_

ah hmm, I guess we only want the first of these here.

---

_@AlexWaygood reviewed on 2025-12-14 16:51_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:490 on 2025-12-14 16:51_

it turns out we were previously incorrectly adding a `foo=` keyword-argument completion suggestion for things like this:

```py
x = (lambda foo: 1 + f<CURSOR>)(42)
```

(inserting `foo=` into the code there doesn't make any sense: a `foo` _variable_ is in scope, but we're not actually calling the `lambda` at this point, or indeed any function, so we shouldn't be offering any keyword-argument completion suggestions.)

The reason for the error is that the cursor here is inside a call expression (so `signature_help()` returns `Some()`), but we weren't checking whether the cursor was inside the `ast::Arguments` part of the call expression. We didn't spot this error in our tests because we previously rendered keyword-argument completion labels the same was a variable completion labels

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:490 on 2025-12-15 08:09_

Could this check be moved inside `signature_help` instead, where we resolve the `covering_node` already.

---

_@MichaReiser reviewed on 2025-12-15 08:09_

Nice find

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:503 on 2025-12-15 13:41_

So I think the root cause of the bug here is that we weren't always using the `insert` text, which _does_ include the `=` suffix here. And this led to sub-optimal results in other cases (like when we want to insert a qualified import).

I actually have a fix for this in on-going work. I can try to split part of that out and also work in your fix for the `lambda` case (nice catch there).

---

_@BurntSushi reviewed on 2025-12-15 13:41_

---

_@AlexWaygood reviewed on 2025-12-15 13:49_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:503 on 2025-12-15 13:49_

Oh, sure! Feel free to close this if it's a bigger issue than I realised and there's a more holistic fix to be done elsewhere :-)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:490 on 2025-12-15 14:46_

This is done in https://github.com/astral-sh/ruff/pull/21988 (which also moves the check to `signature_help`).

---

_@BurntSushi reviewed on 2025-12-15 14:46_

---

_Comment by @AlexWaygood on 2025-12-15 15:51_

closing in favour of #21988

---

_Closed by @AlexWaygood on 2025-12-15 15:51_

---

_Branch deleted on 2025-12-15 15:51_

---
