```yaml
number: 16485
title: "[syntax-errors] Tuple unpacking in `return` and `yield` before Python 3.8"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-unpack-return-yield
created_at: 2025-03-03T23:40:14Z
updated_at: 2025-03-06T16:57:22Z
url: https://github.com/astral-sh/ruff/pull/16485
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Tuple unpacking in `return` and `yield` before Python 3.8

---

_Pull request opened by @ntBre on 2025-03-03 23:40_

Summary
--

Checks for tuple unpacking in `return` and `yield` statements before Python 3.8, as described [here].

Test Plan
--
Inline tests.

[here]: https://github.com/python/cpython/issues/76298


---

_Label `parser` added by @ntBre on 2025-03-03 23:40_

---

_Label `preview` added by @ntBre on 2025-03-03 23:40_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 23:40_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 23:40_

---

_Comment by @github-actions[bot] on 2025-03-03 23:57_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:471 on 2025-03-04 07:56_

Python also knows yield expressions. Is this something we have to distinguish / consider in this PR?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:303 on 2025-03-04 07:57_

Does this also handle nested yield expressions `a = yield x` 
Could this code be moved into `parse_yield_expression`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:405 on 2025-03-04 08:01_

Are the `yield` examples relevant to this code? It seems to me that this parse rule is only used for `return x` or `return yield`.

---

_@MichaReiser reviewed on 2025-03-04 08:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:395 on 2025-03-04 11:31_

Can we add test cases using single unpack element like `return *rest` or `yield *rest`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:395 on 2025-03-04 11:36_

What about `yield 1, (*rest)`, `yield 1, (yield 2, *rest), 3` (yield can be used as an expression)? I'm just throwing out random examples :)

---

_@dhruvmanila reviewed on 2025-03-04 11:37_

---

_@ntBre reviewed on 2025-03-04 14:45_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:405 on 2025-03-04 14:45_

No, you're right. I was getting alarmed at the size of the diffs in these PRs and saw that grouping tests together made them smaller, but conceptually they are separate tests. I'll move those to the `yield` section.

---

_@ntBre reviewed on 2025-03-04 22:16_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:471 on 2025-03-04 22:16_

I'll have to test this out. The CPython PR said statements, but maybe I took it too literally.

---

_@ntBre reviewed on 2025-03-05 14:10_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:471 on 2025-03-05 14:10_

It looks like this also applies to expressions:

Python 3.7:
```pycon
>>> def i(): yield 1, (yield 2, *rest), 3
  File "<stdin>", line 1
    def i(): yield 1, (yield 2, *rest), 3
                                ^
SyntaxError: invalid syntax
```

---

_@ntBre reviewed on 2025-03-05 14:34_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:395 on 2025-03-05 14:34_

Thanks for these! The single cases are actually still invalid on my system Python (3.13):

```pycon
>>> def foo(): return *rest
  File "<python-input-0>", line 1
    def foo(): return *rest
                      ^^^^^
SyntaxError: can't use starred expression here
>>> def foo(): yield *rest
  File "<python-input-1>", line 1
    def foo(): yield *rest
                     ^^^^^
SyntaxError: can't use starred expression here
```

But these aren't flagged as `ParseError`s by ruff.

The second case is very interesting. It's actually accepted on both 3.7 *and* 3.8, but not on my 3.13 system Python (`SyntaxError: cannot use starred expression here`). That might be another change not currently tracked in #6591.

I added your third case as an example of a yield expression and moved the check into `parse_yield_expression` as @MichaReiser suggested.

---

_Comment by @ntBre on 2025-03-05 14:56_

I added more test cases (thanks for the suggestions!). Two of them became TODOs because I think they aren't exactly related to this PR:
* `return|yield *rest` is invalid on every Python version I tried and should be flagged as a *parse* error, in my opinion
* `return|yield 1, (*rest)` is valid on 3.7 and 3.8 but not 3.13, so I think this is a separate change from the 3.8 unpacking (and a second `Change::Removed` case to use the enum from the other PR)

I also checked on `yield from` and it looks like our [`parse_yield_from`](https://github.com/astral-sh/ruff/blob/d0623888b3231fdda09bfa7079d67f8162faa10c/crates/ruff_python_parser/src/parser/expression.rs#L2110) method already reports an error for any unparenthesized tuple expression, so I think that should be covered.

Finally, I changed the message for the `yield statement` to `yield expression`. I think in an ideal case we'd have a third variant of `StarTupleKind` and differentiate `YieldExpr` from `YieldStmt` but I didn't see an easy way to do that with the new detection in `parse_yield_expression`. Since I had to pick one, I thought "expression" was slightly preferable.

---

_Comment by @MichaReiser on 2025-03-05 16:15_

> return|yield *rest is invalid on every Python version I tried and should be flagged as a parse error, in my opinion

Yes, that seems correct. Can you create an issue for it?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:509 on 2025-03-06 07:53_

Nit: I'd add an empty line above comments this long to make it easier to see the different variants.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:460 on 2025-03-06 07:54_

These comments are very helpful! Thanks for taking the time to write them

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2118 on 2025-03-06 07:59_

I'd remove this, considering that it's not a parse error

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2123 on 2025-03-06 08:02_

If you haven't done so. Can you add it to the syntax check issue?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:415 on 2025-03-06 08:03_

Same: Let's remove this as it is a semantic error and not a parse error

---

_@MichaReiser approved on 2025-03-06 08:03_

---

_@dhruvmanila reviewed on 2025-03-06 09:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:395 on 2025-03-06 09:38_

> But these aren't flagged as `ParseError`s by ruff.

That's because the grammar says it's allowed.

```
return_stmt:
    | 'return' [star_expressions] 

star_expressions:
    | star_expression (',' star_expression )+ [','] 
    | star_expression ',' 
    | star_expression

star_expression:
    | '*' bitwise_or 
    | expression
```
I think it's caught by the CPython compiler and not the parser. You can see that `python -m ast test.py` wouldn't raise a syntax error here.

```console
$ python -m ast _.py       
Module(
   body=[
      FunctionDef(
         name='foo',
         args=arguments(
            posonlyargs=[],
            args=[
               arg(arg='args')],
            kwonlyargs=[],
            kw_defaults=[],
            defaults=[]),
         body=[
            Return(
               value=Starred(
                  value=Name(id='args', ctx=Load()),
                  ctx=Load()))],
         decorator_list=[],
         type_params=[])],
   type_ignores=[])
```

---

_@dhruvmanila reviewed on 2025-03-06 09:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:395 on 2025-03-06 09:40_

Oops, missed the issue https://github.com/astral-sh/ruff/issues/16520

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/resources/inline/err/iter_unpack_return_todo.py`:3 on 2025-03-06 09:45_

Oh, interesting!

---

_@dhruvmanila approved on 2025-03-06 09:45_

---

_@ntBre reviewed on 2025-03-06 13:38_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:2118 on 2025-03-06 13:38_

Oh oops, I'll clean up all of these TODOs.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:2123 on 2025-03-06 14:08_

Added to #6591! It looks like this stopped parsing in 3.9

---

_@ntBre reviewed on 2025-03-06 14:08_

---

_Merged by @ntBre on 2025-03-06 16:57_

---

_Closed by @ntBre on 2025-03-06 16:57_

---

_Branch deleted on 2025-03-06 16:57_

---
