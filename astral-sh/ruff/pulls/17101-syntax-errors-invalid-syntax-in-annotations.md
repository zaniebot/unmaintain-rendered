```yaml
number: 17101
title: "[syntax-errors] Invalid syntax in annotations"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-annotation-scopes
created_at: 2025-03-31T20:35:42Z
updated_at: 2025-04-04T12:30:21Z
url: https://github.com/astral-sh/ruff/pull/17101
synced_at: 2026-01-12T15:56:00Z
```

# [syntax-errors] Invalid syntax in annotations

---

_@ntBre_

Summary
--

This PR detects the use of invalid syntax in annotation scopes, including
`yield` and `yield from` expressions and named expressions. I combined a few
different types of CPython errors here, but I think the resulting error messages
still make sense and are even preferable to what CPython gives. For example, we
report `yield expression cannot be used in a type annotation` for both of these:

```pycon
>>> def f[T](x: (yield 1)): ...
  File "<python-input-26>", line 1
    def f[T](x: (yield 1)): ...
                 ^^^^^^^
SyntaxError: yield expression cannot be used within the definition of a generic
>>> def foo() -> (yield x): ...
  File "<python-input-28>", line 1
    def foo() -> (yield x): ...
                  ^^^^^^^
SyntaxError: 'yield' outside function
```

Fixes https://github.com/astral-sh/ruff/issues/11118.

Test Plan
--

New inline tests, along with some updates to existing tests.

---

_Label `rule` added by @ntBre on 2025-03-31 20:35_

---

_Label `preview` added by @ntBre on 2025-03-31 20:35_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-31 20:35_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-31 20:35_

---

_Comment by @github-actions[bot] on 2025-03-31 20:44_

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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:187 on 2025-03-31 21:31_

Is this check to make the `ParseError` and `SemanticSyntaxError` mutually exclusive in the snapshot output? What's the reason for doing this?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:186 on 2025-03-31 21:37_

Do we need to check `type_params.is_none()` in this context?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:165 on 2025-03-31 21:40_

I know this rule is specific to annotations but I was wondering if we need to run the same check for the default value? I'm mainly asking because this PR also checks the default value in type params (`type X[T = (yield 1)] = int`).

---

_@dhruvmanila reviewed on 2025-03-31 21:41_

---

_@ntBre reviewed on 2025-03-31 21:52_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/fixtures.rs`:187 on 2025-03-31 21:52_

Yeah, I thought that would lead to a cleaner diff and be more similar to how the linter reports errors. 9 snapshots change without this, which isn't as bad as I thought, so I'm happy to revert this if you'd prefer.

---

_@ntBre reviewed on 2025-03-31 21:57_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:186 on 2025-03-31 21:57_

This one is actually invalid even without type parameters:

```pycon
>>> type Y = (x := 1)
  File "<python-input-95>", line 1
    type Y = (x := 1)
              ^^^^^^
SyntaxError: named expression cannot be used within a type alias
```

But I did add this as a test case now, thanks!

---

_@ntBre reviewed on 2025-03-31 22:05_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:165 on 2025-03-31 22:05_

You're right that this is a syntax error, but it looks like we already catch these for `yield` expressions: [playground](https://play.ruff.rs/ec306878-cf4a-439c-9333-9df2f3768c17), and they seem to be okay for named expressions:

```pycon
>>> def foo(x=(y := [])): ...
... def bar(x=(y := [])): ...
...
>>>
```

I guess these are currently caught in the parser, so we could move the detection here if we wanted and use `visitor.visit_parameters`, though.

---

_@dhruvmanila reviewed on 2025-04-01 00:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:187 on 2025-04-01 00:25_

I think it should be fine to have both `ParseError` and `SemanticSyntaxError` in the snapshots as it also gives us test coverage for semantic syntax errors in source code containing parse errors.

---

_@dhruvmanila reviewed on 2025-04-01 00:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:165 on 2025-04-01 00:30_

Can you specify which errors are caught in the parser? The playground link that you've provided suggest that the yield expressions are part of the lint rule while the named expressions are fine.

Regardless, we also visit the type param default in here which makes me think whether we should instead split the rule as "InvalidYieldExpressionUsage" and "InvalidNamedExpressionUsage" similar to the variants on `ParseErrorType`?

---

_@ntBre reviewed on 2025-04-01 01:36_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:165 on 2025-04-01 01:36_

> Can you specify which errors are caught in the parser? The playground link that you've provided suggest that the yield expressions are part of the lint rule while the named expressions are fine.

Oh you're right, I just assumed it was a `ParseError` when I saw a diagnostic, but I see now that it's lint rule F704. I guess that's another rule I should reimplement, like [F404](https://github.com/astral-sh/ruff/pull/16106), but I'll save that for a follow-up.

> Regardless, we also visit the type param default in here which makes me think whether we should instead split the rule as "InvalidYieldExpressionUsage" and "InvalidNamedExpressionUsage" similar to the variants on `ParseErrorType`?

Ah, I see what you mean. I guess I thought type parameter defaults were close enough to "type annotations" that the current error message made sense, but I see now that they are pretty different. That's also in line with CPython's different error message:

```pycon
>>> type X[T= (yield 1)] = int
  File "<python-input-112>", line 1
    type X[T= (yield 1)] = int
               ^^^^^^^
SyntaxError: yield expression cannot be used within a TypeVar default
```

Is it enough to split them into one variant for `yield` and one for named expressions, or should we try to split them up more like CPython?


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:831 on 2025-04-01 07:32_

I'm not 100% sure but using the term `type annotation` in a type alias seems misleading because the value isn't a type annotation. The same applies to when using a yield expression in a type var default. We could use a more generic term (e.g. yield expression cannot be used in a type expression) or we distinguish the different contexts. I'm leaning towards the latter.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_annotation_class.py.snap`:420 on 2025-04-01 07:33_

I think the error here is misleading because `y := list)` isn't a type annotation.

---

_@MichaReiser approved on 2025-04-01 07:35_

This overall looks good to me but I think we should improve the error message because some usages of `yield` (or named) aren't in type annotations

---

_Comment by @ntBre on 2025-04-01 14:31_

I'm not sure the names I picked are ideal, but I think the error messages are much more accurate now. The only divergence from CPython, as far as I know, is in cases like this:

```pycon
>>> def foo(arg: yield int): ...
  File "<python-input-154>", line 1
    def foo(arg: yield int): ...
                 ^^^^^
SyntaxError: invalid syntax
```

where CPython reports a very generic "invalid syntax" error. I instead call these "type annotations" like before, which I think is accurate here.

I also followed CPython in having separate messages for generic classes and functions:

```pycon
>>> class F[T]((yield 1)): ...
  File "<python-input-158>", line 1
    class F[T]((yield 1)): ...
                ^^^^^^^
SyntaxError: yield expression cannot be used within the definition of a generic
>>> class F((yield 1)): ...
  File "<python-input-159>", line 1
    class F((yield 1)): ...
             ^^^^^^^
SyntaxError: 'yield' outside function
```

but it might make more sense to use the message "yield expression cannot be used within a base class" for both of these.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:713 on 2025-04-02 16:50_

Does this need to be public or can it be `pub(crate)`? I'd prefer it to be latter.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:889 on 2025-04-02 16:53_

nit: I think "position" might be more suitable here, what do you think?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:165 on 2025-04-02 16:57_

> Is it enough to split them into one variant for `yield` and one for named expressions, or should we try to split them up more like CPython?

I think what you have right now looks great. I only suggested to split them up if we didn't want to provide granular context but I think providing them seems more useful.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_annotation_class.py.snap`:429 on 2025-04-02 17:03_

The "within a base class" sounds a bit confusing to me, what about "cannot be used as a base class"

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_annotation_class.py.snap`:420 on 2025-04-02 17:03_

Should we keep the message same as the one without the type params as the context is still the same? I might be missing something here.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:730 on 2025-04-02 17:05_

Should we spell out the AST nodes instead of specifying their names? For example, using "type variable" instead of "TypeVar", "parameter specification" instead of "ParamSpec"? I don't have any strong preference and fine moving with what you have as well as it matches CPython's error messages.

---

_@dhruvmanila approved on 2025-04-02 17:06_

Looks good! I like the new error messages.

All of the review comments does not need to be addressed, they're more of a suggestion, so feel free to ignore them.

---

_@ntBre reviewed on 2025-04-03 14:31_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:713 on 2025-04-03 14:31_

I think it needs to be `pub` because it's included in a variant of `SemanticSyntaxErrorKind`, which is also `pub`. Clippy gives me a warning if I make it `pub(crate)`, but I would otherwise agree with you.

---

_@ntBre reviewed on 2025-04-03 14:34_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:889 on 2025-04-03 14:34_

Ah yes, that sounds better, thanks! I would usually go for `Kind`, but I already had an `InvalidExpressionKind` here :laughing: 

---

_@ntBre reviewed on 2025-04-03 14:50_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_annotation_class.py.snap`:420 on 2025-04-03 14:50_

This one is actually specific to the case of type params, unlike `yield`:

```pycon
>>> class F[T](y := list): ...
  File "<python-input-0>", line 1
    class F[T](y := list): ...
               ^^^^^^^^^
SyntaxError: named expression cannot be used within the definition of a generic
>>> class F(y := list): ...
...
```

The `yield` cases could probably be combined, but I think I tested them all to be consistent with CPython for now, which gives a very generic error without generics:

```pycon
>>> def n[T]() -> (yield from 1): ...
  File "<python-input-14>", line 1
    def n[T]() -> (yield from 1): ...
                   ^^^^^^^^^^^^
SyntaxError: yield expression cannot be used within the definition of a generic
>>> def n() -> (yield from 1): ...
  File "<python-input-15>", line 1
    def n() -> (yield from 1): ...
                ^^^^^^^^^^^^
SyntaxError: 'yield from' outside function
```

I don't feel very strongly either way. I like consistency with CPython, but I think our non-generic messages are already more helpful and we could easily reuse them like you said.

---

_@ntBre reviewed on 2025-04-03 14:56_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:730 on 2025-04-03 14:56_

I kind of like the type names personally because they feel easier to find in documentation (I'm not sure I'd ever seen "parameter specification" written out haha), but I don't feel strongly either.

It looks like [pyright](https://pyright-play.net/?pythonVersion=3.8&strict=true&code=CYUwZgBA7g2gVHAKgZwgXggCgJ4EsQA2wEAjAJQC6mZAXBAHSNA) refers to them as type parameters or type variables as a group but uses the type names for individual errors, as another data point.

---

_@dhruvmanila reviewed on 2025-04-03 21:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:730 on 2025-04-03 21:30_

Let's go with what you have right now.

---

_@dhruvmanila reviewed on 2025-04-03 21:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@invalid_annotation_class.py.snap`:420 on 2025-04-03 21:32_

I see, thanks for the details. Let's go with what you have right now.

---

_Merged by @ntBre on 2025-04-03 21:56_

---

_Closed by @ntBre on 2025-04-03 21:56_

---

_Branch deleted on 2025-04-03 21:56_

---

_Comment by @JelleZijlstra on 2025-04-04 03:34_

> but it might make more sense to use the message "yield expression cannot be used within a base class" for both of these

That would be inaccurate; you can have a yield expression (or indeed any expression) in a class base.

```
>>> def f():
...     class X((yield 1)):
...         pass
...     yield X
... gen = f()
... gen.send(None)
... x = gen.send(int)
... print(x)
... 
<class '__main__.f.<locals>.X'>
```

It needs to be parenthesized for grammar reasons though.

(Sorry for the post-merge comment; only saw this when the issue was closed.)

---

_Comment by @ntBre on 2025-04-04 12:30_

No worries on the post-merge review, I appreciate it!

That makes a lot of sense, now that you mention it... I think this needs more work in that case, and we likely need to implement the `'yield' outside function` error from CPython separately rather than folding it in here.

---
