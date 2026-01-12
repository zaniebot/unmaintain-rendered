```yaml
number: 21703
title: "parser: remove invalid node from empty & unclosed container types"
type: pull_request
state: closed
author: 11happy
labels:
  - parser
assignees: []
base: main
head: parser_recovery_remove_invalid_node
created_at: 2025-11-30T13:41:36Z
updated_at: 2025-12-14T09:48:33Z
url: https://github.com/astral-sh/ruff/pull/21703
synced_at: 2026-01-12T15:57:31Z
```

# parser: remove invalid node from empty & unclosed container types

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR removes the invalid expression node for empty and unclosed container types. Part of #10653 

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @MichaReiser by @11happy on 2025-11-30 13:41_

---

_Review requested from @dhruvmanila by @11happy on 2025-11-30 13:41_

---

_Comment by @astral-sh-bot[bot] on 2025-11-30 13:59_


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

_Label `parser` added by @AlexWaygood on 2025-11-30 15:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2014 on 2025-12-01 10:16_

Can't we ove the return here? 



---

_@MichaReiser reviewed on 2025-12-01 10:16_

---

_@11happy reviewed on 2025-12-02 06:28_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:2014 on 2025-12-02 06:28_

`NEWLINE_EOF_SET`  contains `NewLine` & `EndofFile` Token , but in these cases with empty & unclosed the first token encountered is Unknown  after  `Lbrace`


---

_@MichaReiser reviewed on 2025-12-05 08:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1950 on 2025-12-05 08:24_

While this works for `[` at the end of a file, it doesn't help if we have something like this


```py
a = [

if True:
	pass
```

I think we want to use something like `if self.at_sequence_end() {` or similar instead.

---

_@11happy reviewed on 2025-12-08 09:37_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:1950 on 2025-12-08 09:37_

done ,
Thank you : )

---

_@MichaReiser reviewed on 2025-12-08 09:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1950 on 2025-12-08 09:39_

Please add new tests demonstrating that this now works as expected

---

_@11happy reviewed on 2025-12-08 09:43_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:1950 on 2025-12-08 09:43_

sure


---

_Review requested from @carljm by @11happy on 2025-12-08 10:57_

---

_Review requested from @AlexWaygood by @11happy on 2025-12-08 10:57_

---

_Review requested from @sharkdp by @11happy on 2025-12-08 10:57_

---

_Review requested from @dcreager by @11happy on 2025-12-08 10:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 11:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 11:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 38 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5519 diagnostics
+ Found 5518 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@11happy reviewed on 2025-12-08 11:15_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:1950 on 2025-12-08 11:15_

done, I have added this test
```
a = [

if True:
	pass
```
should I add similar in case of set/dict etc


---

_@11happy reviewed on 2025-12-08 11:16_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:1950 on 2025-12-08 11:16_

also are the snapshot changes expected ones ? can you please take a look.
Thank you

---

_@AlexWaygood reviewed on 2025-12-08 11:29_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:2205 on 2025-12-08 11:29_

Please remove the TODO comment a few lines above this — this PR fixes the TODO!

---

_@11happy reviewed on 2025-12-08 11:32_

---

_Review comment by @11happy on `crates/ty_ide/src/completion.rs`:2205 on 2025-12-08 11:32_

done

---

_Review request for @carljm removed by @carljm on 2025-12-08 18:43_

---

_Review request for @sharkdp removed by @sharkdp on 2025-12-09 08:33_

---

_Comment by @11happy on 2025-12-14 08:28_

ping!
would appreciate a review : )

---

_@MichaReiser reviewed on 2025-12-14 09:27_

Thank you. We would still add way more tests here, e.g. with an example where a `while` statement or similar follows. 

After working on https://github.com/astral-sh/ruff/pull/21895 I came to the conclusion that we shouldn't make this change because it will result in the lexer and parser state diverging after the missing `]` because we never call `re_lex`.

Fixing this will require a more holistic take at https://github.com/astral-sh/ruff/issues/21896

But thank you for looking into this

---

_Closed by @MichaReiser on 2025-12-14 09:27_

---

_Comment by @11happy on 2025-12-14 09:48_

> Thank you. We would still add way more tests here, e.g. with an example where a `while` statement or similar follows.
> 
> After working on #21895 I came to the conclusion that we shouldn't make this change because it will result in the lexer and parser state diverging after the missing `]` because we never call `re_lex`.
> 
> Fixing this will require a more holistic take at #21896
> 
> But thank you for looking into this

Sure, I understand.
Thank you

---
