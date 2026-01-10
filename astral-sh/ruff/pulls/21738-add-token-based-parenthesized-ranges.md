```yaml
number: 21738
title: "Add token based `parenthesized_ranges` implementation"
type: pull_request
state: merged
author: denyszhak
labels:
  - internal
assignees: []
merged: true
base: main
head: feat-optimized-parenthesized-range
created_at: 2025-12-01T20:18:04Z
updated_at: 2025-12-03T09:52:25Z
url: https://github.com/astral-sh/ruff/pull/21738
synced_at: 2026-01-10T16:48:02Z
```

# Add token based `parenthesized_ranges` implementation

---

_Pull request opened by @denyszhak on 2025-12-01 20:18_

## Summary

Optimizes parenthesized_range by using token slice partitioning to reduce scanning overhead.

Closes #21510 

## Test Plan

1. Added unit tests for parenthesized_range and parentheses_iterator
2. Added coverage to ensure behavior matches the previous implementation

Acknowledging closed PR #21734 by @FarhanAliRaza for a couple of insights that helped shape the final version.

---

_Review requested from @carljm by @denyszhak on 2025-12-01 20:18_

---

_Review requested from @AlexWaygood by @denyszhak on 2025-12-01 20:18_

---

_Review requested from @sharkdp by @denyszhak on 2025-12-01 20:18_

---

_Review requested from @dcreager by @denyszhak on 2025-12-01 20:18_

---

_Review requested from @MichaReiser by @denyszhak on 2025-12-01 20:18_

---

_Review requested from @dhruvmanila by @denyszhak on 2025-12-01 20:18_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-01 20:19_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 20:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 20:31_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics

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

_Comment by @astral-sh-bot[bot] on 2025-12-01 20:46_


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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:86 on 2025-12-02 08:36_

I would add the method to `ruff_python_ast`. Ideally (not as part of this PR), we'd also move `Tokens` out of `ruff_python_parser` so that crates that only process the AST but don't care about how it was created don't have to depend on the parser crate. 

I suggest moving this module into `ruff_python_ast::tokens` 


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parenthesize.rs`:16 on 2025-12-02 08:38_

```suggestion
            | TokenKind::NonLogicalNewline
```

I think we should remove `Dedent`, `Indent`, and `Newline` as all of those aren't trivia. Instead, they have semantic meaning in Python. Are there cases where you had to skip over those to make the two functions behave the same?

If they aren't needed for correctness, you can then use `token.kind().is_trivia()`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parenthesize.rs`:50 on 2025-12-02 08:49_

Nit: You can rewrite it like this to keep it closer to the original implementation:

```suggestion
    let after_tokens = if let Some(parent) = parent {
        // If the parent is a node that brings its own parentheses, exclude the closing parenthesis
        // from our search range. Otherwise, we risk matching on calls, like `func(x)`, for which
        // the open and close parentheses are part of the `Arguments` node.
        let exclusive_parent_end = if parent.is_arguments() {
            parent.end() - ")".text_len()
        } else {
            parent.end()
        };

        tokens.in_range(TextRange::new(expr.end(), exclusive_parent_end))
    } else {
        tokens.after(expr.end())
    };
    
    let right_parens = after_tokens
        .iter()
        .filter(|token| !token.kind().is_trivia())
        .take_while(move |token| token.kind() == TokenKind::Rpar);
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parenthesize.rs`:74 on 2025-12-02 08:50_

Nice!

---

_Review comment by @MichaReiser on `crates/ruff_python_ast_integration_tests/tests/parenthesize_optimized.rs`:1 on 2025-12-02 08:51_

I can see how those tests were useful during development to gain a better understanding of how the existing implementation works but I'm inclined to remove the tests now that you have a working implementation (or make them unit tests of `optimized_parenthesize`)

---

_@MichaReiser requested changes on 2025-12-02 08:52_

Nice, this looks great. I've a few smaller nit comments but it's mostly good to go

---

_Label `internal` added by @MichaReiser on 2025-12-02 08:52_

---

_@denyszhak reviewed on 2025-12-02 14:03_

---

_Review comment by @denyszhak on `crates/ruff_python_parser/src/lib.rs`:86 on 2025-12-02 14:03_

New method depends on Tokens, which currently lives in ruff_python_parser. If we move that method into ruff_python_ast without also moving Tokens, then ruff_python_ast would need to depend on ruff_python_parser, creating a circular dependency. Does that sound reasonable or am I interpreting it wrong?

---

_@denyszhak reviewed on 2025-12-02 14:04_

---

_Review comment by @denyszhak on `crates/ruff_python_ast_integration_tests/tests/parenthesize_optimized.rs`:1 on 2025-12-02 14:04_

I'm going to remove it, original unit tests provide broad coverage in general

---

_@MichaReiser reviewed on 2025-12-02 15:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:86 on 2025-12-02 15:55_

Oh hmm, that makes sense. It might be worth moving `Tokens` as a separate PR if you're up to it (I don't know how hard that would be)

---

_@denyszhak reviewed on 2025-12-02 17:13_

---

_Review comment by @denyszhak on `crates/ruff_python_parser/src/lib.rs`:86 on 2025-12-02 17:13_

I would love to attempt it in a separate PR and leave this one in ruff_python_parser if you are ok with that

---

_@denyszhak reviewed on 2025-12-02 17:31_

---

_Review comment by @denyszhak on `crates/ruff_python_ast_integration_tests/tests/parenthesize_optimized.rs`:1 on 2025-12-02 17:31_

done

---

_@denyszhak reviewed on 2025-12-02 17:39_

---

_Review comment by @denyszhak on `crates/ruff_python_parser/src/parenthesize.rs`:16 on 2025-12-02 17:39_

Part of additional kinds I referenced from SimpleTokenKind is_trivia, but part of it from the PR I referenced above thinking I have missed those. It doesn't influence correctness at least for the tests I added.

I changed to using `oken.kind().is_trivia()` as you suggested because now I think mimicking SimpleTokenKind.is_trivia was the wrong move


---

_Comment by @denyszhak on 2025-12-02 21:48_

@MichaReiser this one should be ready for you to review again

---

_Review requested from @MichaReiser by @amyreese on 2025-12-02 23:18_

---

_Renamed from "feat: optimize parenthesized_range" to "Add token based `parenthesized_ranges` implementation" by @MichaReiser on 2025-12-03 08:08_

---

_@MichaReiser approved on 2025-12-03 08:09_

Thanks, this is great.

If you're interested, it would be great if we could rewrite the calls to `parenthesized_range` in `ruff_linter` to use your new implementation

---

_Merged by @MichaReiser on 2025-12-03 08:15_

---

_Closed by @MichaReiser on 2025-12-03 08:15_

---

_Comment by @denyszhak on 2025-12-03 09:52_

> Thanks, this is great.
> 
> If you're interested, it would be great if we could rewrite the calls to `parenthesized_range` in `ruff_linter` to use your new implementation

I'm definitely interested, let me prepare a PR for this, I will create an issue and link it there

---
