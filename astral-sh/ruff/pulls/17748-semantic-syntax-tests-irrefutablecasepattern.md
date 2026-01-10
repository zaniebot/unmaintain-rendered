```yaml
number: 17748
title: "[semantic-syntax-tests] `IrrefutableCasePattern`, `SingleStarredAssignment`, `WriteToDebug`, `InvalidExpression`"
type: pull_request
state: merged
author: maxmynter
labels:
  - testing
assignees: []
merged: true
base: main
head: semantic-test-coverage
created_at: 2025-04-30T21:04:54Z
updated_at: 2025-05-09T18:54:05Z
url: https://github.com/astral-sh/ruff/pull/17748
synced_at: 2026-01-10T18:57:03Z
```

# [semantic-syntax-tests] `IrrefutableCasePattern`, `SingleStarredAssignment`, `WriteToDebug`, `InvalidExpression`

---

_Pull request opened by @maxmynter on 2025-04-30 21:04_

Re: #17526 

## Summary

Add integration test for semantic syntax for `IrrefutableCasePattern`, `SingleStarredAssignment`, `WriteToDebug`, and `InvalidExpression`.

## Notes
- Following @ntBre's suggestion, I will keep the test coming in batches like this over the next few days in separate PRs to keep the review load per PR manageable while also not spamming too many.

- I did not add a test for `del __debug__` which is one of the examples in `crates/ruff_python_parser/src/semantic_errors.rs:1051`. 
For python version `<= 3.8` there is no error and for `>=3.9` the error is not `WriteToDebug` but `SyntaxError: cannot delete __debug__ on Python 3.9 (syntax was removed in 3.9)`.

- The `blacken-docs` bypass is necessary because otherwise the test does not pass pre-commit checks; but we want to check for this faulty syntax.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
This is a test.

---

_Review requested from @carljm by @maxmynter on 2025-04-30 21:04_

---

_Review requested from @AlexWaygood by @maxmynter on 2025-04-30 21:04_

---

_Review requested from @sharkdp by @maxmynter on 2025-04-30 21:04_

---

_Review requested from @dcreager by @maxmynter on 2025-04-30 21:04_

---

_Comment by @github-actions[bot] on 2025-04-30 21:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "(tests) Add linter.rs and red knot integration tests" to "[semantic-syntax-tests] Integration tests in linter and red knot for `IrrefutableCasePattern`, `SingleStarredAssignment`, `WriteToDebug`, and `InvalidExpression`" by @maxmynter on 2025-04-30 21:07_

---

_Renamed from "[semantic-syntax-tests] Integration tests in linter and red knot for `IrrefutableCasePattern`, `SingleStarredAssignment`, `WriteToDebug`, and `InvalidExpression`" to "[semantic-syntax-tests] `IrrefutableCasePattern`, `SingleStarredAssignment`, `WriteToDebug`, `InvalidExpression`" by @maxmynter on 2025-04-30 21:08_

---

_@carljm approved on 2025-04-30 21:19_

red-knot tests look good

---

_Label `testing` added by @ntBre on 2025-04-30 21:28_

---

_Comment by @github-actions[bot] on 2025-04-30 21:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:274 on 2025-05-01 15:43_

I think if you parenthesize the `yield from` you can get by without the `blacken-docs:off`:

```suggestion
class C((yield from [object])):
```

Without the parens, it's actually a parse error in CPython, rather than a semantic/compiler error. I'm surprised we don't emit an error from the parser too.

```pycon
>>> import ast
>>> ast.parse("class C(yield from [object]): ...")
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    ast.parse("class C(yield from [object]): ...")
    ~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.13/ast.py", line 54, in parse
    return compile(source, filename, mode, flags,
                   _feature_version=feature_version, optimize=optimize)
  File "<unknown>", line 1
    class C(yield from [object]): ...
            ^^^^^
SyntaxError: invalid syntax
>>> ast.parse("class C((yield from [object])): ...")
<ast.Module object at 0x70a940be1c90>
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1095 on 2025-05-01 15:44_

nit: I'd probably put these on one line if they fit, but it's not a big deal either way.

```suggestion
        "*a = [1, 2, 3, 4]",
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__semantic_syntax_error_InvalidExpression_invalid_expression_yield_from_in_base_class_3.10.snap`:8 on 2025-05-01 15:46_

Ah okay, we do emit a parse error for this. This one needs parens too to get the semantic error instead.

---

_@ntBre reviewed on 2025-05-01 15:47_

Thanks! I had one suggestion about the `yield from` cases and a nit you can ignore if you prefer it this way.

---

_Review comment by @maxmynter on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__semantic_syntax_error_InvalidExpression_invalid_expression_yield_from_in_base_class_3.10.snap`:8 on 2025-05-02 19:08_

with 

```py
class C((yield from [object])):
    pass
```
results in an empty snapshot for the associated test. 

Am I doing something wrong, is there an issue with the implementation, or is this desired? 

---

_@maxmynter reviewed on 2025-05-02 19:08_

---

_@ntBre reviewed on 2025-05-05 15:00_

---

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__semantic_syntax_error_InvalidExpression_invalid_expression_yield_from_in_base_class_3.10.snap`:8 on 2025-05-05 15:00_

Oh, this is actually allowed in CPython with the parens (at least if you enclose it in a function to avoid _that_ error):

```pycon
>>> class C((yield from [object])):
...     pass
...
  File "<python-input-2>", line 1
    class C((yield from [object])):
             ^^^^^^^^^^^^^^^^^^^
SyntaxError: 'yield from' outside function
>>> class C((yield from [object])):
...     pass
KeyboardInterrupt
>>> def f():
...     class C((yield from [object])): ...
...
>>>
```

So I guess I would say just remove this test case, or make it generic, which _should_ cause an error:

```pycon
>>> def f():
...     class C[T]((yield from [object])): ...
...
  File "<python-input-4>", line 2
    class C[T]((yield from [object])): ...
                ^^^^^^^^^^^^^^^^^^^
SyntaxError: yield expression cannot be used within the definition of a generic
```

---

_Comment by @maxmynter on 2025-05-06 22:01_

I went with the generic type parameter and pinned the necessary Python version, `3.12`. Also rebased on top of main and resolved merge conflicts that came from the other PRs adding tests. 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-06 22:17_

---

_Review requested from @ntBre by @maxmynter on 2025-05-06 22:22_

---

_Comment by @maxmynter on 2025-05-06 22:23_

If this is good, it also closes #17526,  I guess. 

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1202 on 2025-05-07 18:48_

```suggestion
        class C[T]((yield from [object])):
```

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:314 on 2025-05-07 18:51_

Sorry for not noticing this before, but these need to be `py` not `python` blocks or the mdtests don't actually run. The same is true for the `await` outside async function block below. I think those are the only 3 places, though. I was alerted to this because the error message should be `yield expression cannot be used within a generic definition`. I didn't explicitly handle `yield from` for the message, although I probably should have!

---

_@ntBre reviewed on 2025-05-07 18:52_

Thanks! I was just going to push the small tweak to the parens in the ruff case, but then I noticed the mdtest code block issue. Could you update those? Then this should be ready to go.

---

_@maxmynter reviewed on 2025-05-07 22:41_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/linter.rs`:1202 on 2025-05-07 22:41_

Damn, I must have undone this somehow during rebasing; because I remember having added these. Re-added in [94d6aee](https://github.com/astral-sh/ruff/pull/17748/commits/94d6aee4ddd515320befb8e855b954ee1c4a17c9)

---

_@maxmynter reviewed on 2025-05-07 22:45_

---

_Review comment by @maxmynter on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:248 on 2025-05-07 22:45_

This one was not thrown after the `python` -> `py` change. Is that a reason for concern @ntBre? Line was added in [e7f38fe](https://github.com/astral-sh/ruff/commit/e7f38fe74ba642cbd4cf286d8524f4172cbe30e2)

---

_Review requested from @ntBre by @maxmynter on 2025-05-07 22:52_

---

_@maxmynter reviewed on 2025-05-07 22:54_

---

_Review comment by @maxmynter on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:248 on 2025-05-07 22:54_

A couple other errors were thrown due to this change. I adapted the test cases because they seem reasonanle. Let me know if you disagree.

---

_@ntBre reviewed on 2025-05-08 14:05_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:248 on 2025-05-08 14:05_

Wow one of the `python` blocks was mine! Thanks for the fix.

And yes, that error should not be thrown. I fixed it in a different PR and mentioned it on the PR adding this case [here](https://github.com/astral-sh/ruff/pull/17463#discussion_r2050785521).

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:303 on 2025-05-08 14:09_

nit: I'd probably just wrap this in an outer function to suppress these `outside of a function` errors since we only intend to test the other errors here. I think a lot of other mdtests use a function named `_` to indicate this:

```py
def _():
    type X[T: (yield 1)] = int
```

but it's not a huge deal.

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:307 on 2025-05-08 14:10_

This doesn't necessarily need to block this PR since you're just reporting the current state of things, but we might need to open a follow-up issue here to avoid duplicate diagnostics. @AlexWaygood do you know if the type error is separate from the syntax error here, or should we only pick one of these to emit?

---

_@ntBre approved on 2025-05-08 14:14_

Thanks again! The new errors prompted one question, mostly for Alex or someone else on the ty team. I also had one suggestion about the "outside of a function" errors, but this is just about ready to go.

---

_@maxmynter reviewed on 2025-05-09 16:40_

---

_Review comment by @maxmynter on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:303 on 2025-05-09 16:40_

I think there is still something here. With 
```py
def _():
# error: [invalid-type-form] "`yield` expressions are not allowed in type expressions"
# error: [invalid-syntax] "yield expression cannot be used within a TypeVar bound"
    type X[T: (yield 1)] = int

def _():
# error: [invalid-type-form] "`yield` expressions are not allowed in type expressions"
# error: [invalid-syntax] "yield expression cannot be used within a type alias"
    type Y = (yield 1)
```

I still get the outside of a function errors

```bash
semantic_syntax_errors.md - Semantic syntax error diagnostics - Invalid expression

  crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md:304 unexpected error: 16 [invalid-syntax] "`yield` statement outside of a function"
  crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md:309 unexpected error: 15 [invalid-syntax] "`yield` statement outside of a function"
```

---

_Comment by @maxmynter on 2025-05-09 16:41_

One duplicate test case had slipped in, which i removed in [96aeedc](https://github.com/astral-sh/ruff/pull/17748/commits/96aeedc4b442acc632aa69c39f3403559d2516ed).

---

_Review requested from @ntBre by @maxmynter on 2025-05-09 16:42_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:303 on 2025-05-09 17:22_

Ah, that's a bug, thank you! We can merge this as is, and I'll tackle it in a follow-up.

If you're curious, ty has separate scope kinds for type aliases and annotations, so the check for function scope needs to skip those instead of just checking that the immediate scope is a function. At least that looks like the easy fix to me.

---

_@ntBre reviewed on 2025-05-09 17:22_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:307 on 2025-05-09 17:45_

I talked to Alex, and this is a symptom of a more general need to suppress type-checking errors when there are syntax errors present. I'll spin off an issue for this too, so no worries here.

---

_@ntBre reviewed on 2025-05-09 17:45_

---

_@ntBre approved on 2025-05-09 17:48_

Thanks! And thanks for all of your help on #17526, I'll close that now.

---

_@ntBre reviewed on 2025-05-09 18:00_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:307 on 2025-05-09 18:00_

The issue already exists in the ty repo! https://github.com/astral-sh/ty/issues/119

---

_Merged by @ntBre on 2025-05-09 18:54_

---

_Closed by @ntBre on 2025-05-09 18:54_

---
