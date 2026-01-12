```yaml
number: 21796
title: "[ty] Add autocomplete suggestions for parameters in function calls"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: function-param-completions
created_at: 2025-12-04T19:18:31Z
updated_at: 2025-12-09T05:24:37Z
url: https://github.com/astral-sh/ruff/pull/21796
synced_at: 2026-01-12T15:57:33Z
```

# [ty] Add autocomplete suggestions for parameters in function calls

---

_@RasmusNygren_

This adds autocomplete suggestions for function arguments
 like `okay` in:
```python
def foo(okay=None):

foo(o<CURSOR>
```

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes [#1550](https://github.com/astral-sh/ty/issues/1550)
Adds autocomplete suggestions for function arguments.


## Test Plan
- New tests
- Modify impacted "FIXME" tests
- Testing in the playground


---

_Comment by @astral-sh-bot[bot] on 2025-12-04 19:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review comment by @RasmusNygren on `crates/ty_python_semantic/src/types/signatures.rs`:2295 on 2025-12-04 19:22_

This does change the visibility of `ParameterKind`, I don't know if you see an issue with that but it felt more reasonable to me than duplicating a similar enum elsewhere.

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 19:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5545 diagnostics
+ Found 5544 diagnostics

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

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:449 on 2025-12-04 19:25_

This PR does not add any type information here but if this approach seems reasonable then it would make a lot of sense to also pass down the type information to be able to give better context in the autocomplete suggestions.

---

_@RasmusNygren reviewed on 2025-12-04 19:25_

---

_Marked ready for review by @RasmusNygren on 2025-12-04 19:32_

---

_Review requested from @carljm by @RasmusNygren on 2025-12-04 19:32_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-04 19:32_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-04 19:32_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-04 19:32_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-04 19:32_

---

_Label `server` added by @AlexWaygood on 2025-12-04 19:46_

---

_Label `ty` added by @AlexWaygood on 2025-12-04 19:46_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-04 21:52_

---

_Review request for @carljm removed by @MichaReiser on 2025-12-04 21:52_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-04 21:52_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-12-04 21:52_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-04 21:52_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/signatures.rs`:2295 on 2025-12-04 22:16_

Yeah it's fine. It's practically a meme at this point how often the LSP functionality causes use to export more stuff from `ty_python_semantic`. :-)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:3264 on 2025-12-04 22:24_

Could you add a test that involves multiple function signatures?

And some other cases:

* A test with multiple keyword arguments.
* A test where some keyword arguments are already set.
* A test where the cursor is between two keyword arguments, e.g., `foo(abc=1, d<CURSOR> xyz=1)`
* A test that checks whether we are omitting duplicative keyword arguments. (I don't think this PR does that. But I don't think that's a blocking concern.)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:443 on 2025-12-04 22:25_

Could this be split out into a separate function? There's already a lot going on here.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:449 on 2025-12-04 22:26_

Yeah I think we should include type information. At present, I believe the only case where we don't set `ty` is for auto-import.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:459 on 2025-12-04 22:26_

Should we detect which arguments are already present and filter them out?

---

_@BurntSushi reviewed on 2025-12-04 22:32_

Wow, this is amazing! I actually almost worked on this today. :-)

I think there are probably some more tests that should be added before landing and a few nits, but I think this is a nice improvement on the status quo.

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:3264 on 2025-12-05 20:24_

I think I managed to cover most of your suggestions in new test cases. I went ahead and added the type information and support to filter out arguments that are already set (as per your other comment) so I think it's looking quite decent now :)

---

_@RasmusNygren reviewed on 2025-12-05 20:24_

---

_Review requested from @BurntSushi by @RasmusNygren on 2025-12-05 20:50_

---

_@RasmusNygren reviewed on 2025-12-08 17:43_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:3264 on 2025-12-08 17:43_

As you were onto, the behaviour is still slightly funky when you have something like
```python
def f(foobar: int): ...
foobar = 5
f(foo<CURSOR>
```
As when we de-dupe we are left with the completion `foobar: Literal[5]`. While unintuitive it doesn't seem that trivial to get right in the general case. Maybe we need to add some kind of ranking system or do something a little bit more clever to get this right for the general case (for all kind of completions).

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:3264 on 2025-12-08 18:31_

Yeah I agree. I think this is fine for now.

---

_@BurntSushi reviewed on 2025-12-08 18:31_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:466 on 2025-12-08 18:36_

This should be a `///` comment.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:522 on 2025-12-08 18:36_

Same as above. This should also be a `///` comment.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:524 on 2025-12-08 18:37_

I think this should return a `FxHashSet` so that membership testing is constant time. Otherwise the loop in the caller is quadratic. (Which probably doesn't matter much in most real world cases, and perhaps even faster, but it'd be better to avoid quadratic complexity and this is a simple way to do it.)

---

_@BurntSushi approved on 2025-12-08 18:53_

Nice thank you! This looks awesome. Great tests. :-) I left some comments that I just fixed up myself since they were super minor.

---

_@RasmusNygren reviewed on 2025-12-08 19:13_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:524 on 2025-12-08 19:13_

Yep reasonable, I figured that the list should generally always be quite short so using a set might not even be faster in practice. I did overlook how simple it is to make it set instead though so I do agree that being theoretically sound here is the way to go. Thanks!

---

_Merged by @BurntSushi on 2025-12-08 19:19_

---

_Closed by @BurntSushi on 2025-12-08 19:19_

---
