```yaml
number: 21200
title: "[`flake8-bugbear`] Catch `yield` expressions within other statements (`B901`)"
type: pull_request
state: merged
author: 11happy
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: return_in_async_generator
created_at: 2025-11-02T09:35:28Z
updated_at: 2025-12-03T21:54:00Z
url: https://github.com/astral-sh/ruff/pull/21200
synced_at: 2026-01-12T15:57:18Z
```

# [`flake8-bugbear`] Catch `yield` expressions within other statements (`B901`)

---

_@11happy_

## Summary

This PR re-implements [return-in-generator (B901)](https://docs.astral.sh/ruff/rules/return-in-generator/#return-in-generator-b901) for async generators as a semantic syntax error. This is not a syntax error for sync generators, so we'll need to preserve both the lint rule and the syntax error in this case.

It also updates B901 and the new implementation to catch cases where the generator's `yield` or `yield from` expression is part of another statement, as in:

```py
def foo():
    return (yield)
```

These were previously not caught because we only looked for `Stmt::Expr(Expr::Yield)` in `visit_stmt` instead of visiting `yield` expressions directly. I think this modification is within the spirit of the rule and safe to try out since the rule is in preview.

## Test Plan

<!-- How was it tested? -->
I have written tests as directed in #17412 


---

_Comment by @github-actions[bot] on 2025-11-02 09:37_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-02 09:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
+ WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`

sphinx (https://github.com/sphinx-doc/sphinx)
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`

prefect (https://github.com/PrefectHQ/prefect)
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `ClassLiteral < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`
- WARN expected `heap_size` to be provided by Salsa query `GenericAlias < 'db >::variance_of_`

```
</details>


---

_Comment by @11happy on 2025-11-02 09:43_

Hii @ntBre would appreciate your views on some design decisions for this PR , 
- I am currently detecting return statements like this, 
  ```
  body.iter().any(|stmt| matches!(stmt, Stmt::Return(_)))
  ```
  I think it migh miss the nested ones, should I also use visitor pattern like in original `B901` 
- also if you have any suggestions for the error message, currently kept it short.
- right now its pointing to yield should it point to return ?

Thank you : )

---

_Comment by @github-actions[bot] on 2025-11-02 09:57_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+35 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/paper_fns/auto_git/query_analyzer.py#L292'>crazy_functions/paper_fns/auto_git/query_analyzer.py:292:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/review_fns/query_analyzer.py#L380'>crazy_functions/review_fns/query_analyzer.py:380:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary><a href="https://github.com/agronholm/anyio">agronholm/anyio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/agronholm/anyio/blob/e415faf9df854e559e855970fe0d892792c1abb8/src/anyio/pytest_plugin.py#L145'>src/anyio/pytest_plugin.py:145:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/0ae6e622fb618ca833a6106dedb1aed4884ca94a/src/trio/_core/_traps.py#L69'>src/trio/_core/_traps.py:69:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/assertion/__init__.py#L192'>src/_pytest/assertion/__init__.py:192:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/cacheprovider.py#L298'>src/_pytest/cacheprovider.py:298:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/cacheprovider.py#L419'>src/_pytest/cacheprovider.py:419:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/cacheprovider.py#L461'>src/_pytest/cacheprovider.py:461:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/capture.py#L890'>src/_pytest/capture.py:890:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/capture.py#L895'>src/_pytest/capture.py:895:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/capture.py#L900'>src/_pytest/capture.py:900:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/capture.py#L905'>src/_pytest/capture.py:905:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/config/__init__.py#L1412'>src/_pytest/config/__init__.py:1412:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/debugging.py#L308'>src/_pytest/debugging.py:308:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/faulthandler.py#L102'>src/_pytest/faulthandler.py:102:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/helpconfig.py#L152'>src/_pytest/helpconfig.py:152:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/logging.py#L780'>src/_pytest/logging.py:780:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/logging.py#L788'>src/_pytest/logging.py:788:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/logging.py#L801'>src/_pytest/logging.py:801:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/logging.py#L873'>src/_pytest/logging.py:873:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/pytester.py#L171'>src/_pytest/pytester.py:171:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/setuponly.py#L36'>src/_pytest/setuponly.py:36:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/skipping.py#L268'>src/_pytest/skipping.py:268:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/skipping.py#L312'>src/_pytest/skipping.py:312:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/terminal.py#L717'>src/_pytest/terminal.py:717:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/terminal.py#L981'>src/_pytest/terminal.py:981:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/terminal.py#L992'>src/_pytest/terminal.py:992:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/tmpdir.py#L315'>src/_pytest/tmpdir.py:315:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/unittest.py#L587'>src/_pytest/unittest.py:587:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/warnings.py#L109'>src/_pytest/warnings.py:109:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/warnings.py#L118'>src/_pytest/warnings.py:118:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/warnings.py#L128'>src/_pytest/warnings.py:128:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/warnings.py#L89'>src/_pytest/warnings.py:89:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/src/_pytest/warnings.py#L98'>src/_pytest/warnings.py:98:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/8b3550c6cb4ea3898140df153a83addc8cd65710/testing/conftest.py#L91'>testing/conftest.py:91:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B901 | 35 | 35 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @ntBre on 2025-11-03 18:47_

> Hii @ntBre would appreciate your views on some design decisions for this PR ,
> 
> * I am currently detecting return statements like this,
>   ```
>   body.iter().any(|stmt| matches!(stmt, Stmt::Return(_)))
>   ```
>   
>   I think it migh miss the nested ones, should I also use visitor pattern like in original `B901`
> * also if you have any suggestions for the error message, currently kept it short.
> * right now its pointing to yield should it point to return ?
> 
> Thank you : )


Yes, I think we should use a visitor like in B901. We can't fully replace B901 since it also applies to sync functions, but we should make sure we catch all of the async cases B901 handles.

I think the error message looks good currently and matches CPython, but I think it should point to `return` rather than `yield`, also like CPython and B901.


---

_Marked ready for review by @11happy on 2025-11-07 03:18_

---

_Review requested from @carljm by @11happy on 2025-11-07 03:18_

---

_Review requested from @AlexWaygood by @11happy on 2025-11-07 03:18_

---

_Review requested from @sharkdp by @11happy on 2025-11-07 03:18_

---

_Review requested from @dcreager by @11happy on 2025-11-07 03:18_

---

_Review requested from @MichaReiser by @11happy on 2025-11-07 03:18_

---

_Review requested from @dhruvmanila by @11happy on 2025-11-07 03:18_

---

_Comment by @11happy on 2025-11-07 03:18_

its ready for review : )
Thank you

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-10 12:22_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-10 12:22_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-10 12:22_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-10 12:22_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2025-11-10 12:22_

---

_Review requested from @ntBre by @MichaReiser on 2025-11-10 12:22_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/syntax_errors/return_in_generator.py`:1 on 2025-11-10 17:04_

Could we add a few more test cases here? As a couple of examples, this is not a syntax error for non-async functions:

```py
def gen():
    yield 1
    return 42
```

and it's also not a syntax error for `return` without a value:

```py
async def gen():
    yield 1
    return
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:867 on 2025-11-10 17:09_

Does this need to be a `SemanticSyntaxContext` method? From looking at B901, it seems that it runs on `StmtFunctionDef`s, not on `yield` statements. I think we could do the same in `SemanticSyntaxChecker::visit_stmt`. That would be a bit simpler and we wouldn't have to implement methods for Ruff and ty separately.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1594 on 2025-11-10 17:10_

Could we add some docs here? I think it's especially important to capture the "with value" part since it's not in the name.

---

_@ntBre requested changes on 2025-11-10 17:10_

Thanks! I think we could simplify things slightly by following the B901 implementation more closely.

---

_Label `internal` added by @ntBre on 2025-11-10 17:11_

---

_Comment by @ntBre on 2025-11-10 17:14_

Oh, one other thing I forgot to mention: I think we need to make B901 only run on sync generators and this syntax error only run on async generators. Then we'll need to convert the syntax error into a B901 error for now to preserve backwards compatibility.

---

_@11happy reviewed on 2025-11-12 03:09_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1594 on 2025-11-12 03:09_

done

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/mod.rs`:867 on 2025-11-12 03:10_

done

---

_@11happy reviewed on 2025-11-12 03:10_

---

_@11happy reviewed on 2025-11-12 03:10_

---

_Review comment by @11happy on `crates/ruff_linter/resources/test/fixtures/syntax_errors/return_in_generator.py`:1 on 2025-11-12 03:10_

done added more cases

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/return_in_generator.rs`:99 on 2025-11-12 21:58_

small nit: It might be worth a comment here like:


```suggestion
    // Async functions are flagged by the `ReturnInAsyncGenerator` semantic syntax error.
    if function_def.is_async {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:983 on 2025-11-12 22:00_

nit: I think we should maybe use `PythonVersion::default()` or `PythonVersion::latest()` unless we have a specific reason to choose a version. I think most of our tests run with `latest`.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:840 on 2025-11-12 22:05_

nit: can we revert this?


```suggestion
```

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1769 on 2025-11-12 22:07_

I think it's probably worth copying the comment from B901:


```suggestion
            // Do not recurse into nested functions; they're evaluated separately.
            Stmt::FunctionDef(_) | Stmt::ClassDef(_) => {}
```

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1736 on 2025-11-12 22:10_

I like that this would take the first return like CPython does rather than the last return like B901, but I think for the sake of compatibility with B901, we should just take the last return and always overwrite:


```suggestion
                self.return_range = Some(*range);
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:741 on 2025-11-12 22:13_

We need to convert this into a B901 error right? It looks like we actually don't have any B901 test cases with async functions (that trigger the lint), otherwise I would have expected tests to fail. We should probably add one of those as well.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:16 on 2025-11-12 22:14_

Let's keep this empty line.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:7 on 2025-11-12 22:14_

nit:

```suggestion
use ruff_python_ast::statement_visitor::{self, StatementVisitor};
```

---

_@ntBre reviewed on 2025-11-12 22:16_

Thanks, this is looking great. I just had a few more nits.

---

_Review requested from @ntBre by @11happy on 2025-11-26 11:29_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1727 on 2025-12-01 22:13_

I noticed this while reviewing a different PR (https://github.com/astral-sh/ruff/pull/21177#discussion_r2539702728), but I think this logic is actually incorrect here and in B901 itself. This only catches `yield` and `yield from` in standalone statements, but the syntax error applies even when these expressions are nested within other statements:

```pycon
>>> async def foo():
...     return (yield 1)
...
  File "<python-input-1>", line 2
    return (yield 1)
    ^^^^^^^^^^^^^^^^
SyntaxError: 'return' with value in async generator
```

That's one compact example that won't be flagged here.

The fix is to implement both `visit_stmt` and `visit_expr` and to perform the `yield` checks in `visit_expr`, as in PLR1708:

https://github.com/astral-sh/ruff/blob/0e651b50b7d0c2614c5958e78525213e759e5b41/crates/ruff_linter/src/rules/pylint/rules/stop_iteration_return.rs#L99-L100

We also have to call `walk_stmt` in the `Return` branch, or this case will still not be caught.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1046 on 2025-12-01 22:19_

I think it's okay to leave this in the `test_semantic_errors` test case. It's a bit confusing to see the non-async errors in the snapshot here, especially with the `ok` comments.

I think we should just add 1 or 2 `async` test cases to `B901.py` to make sure the syntax error is being converted correctly for now.

---

_@ntBre requested changes on 2025-12-01 22:21_

Thank you, this is looking great! I just had one more nit about test placement, and I noticed a preexisting error that I think we can fix here too.

---

_@11happy reviewed on 2025-12-02 11:04_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1727 on 2025-12-02 11:04_

done

---

_@11happy reviewed on 2025-12-02 11:04_

---

_Review comment by @11happy on `crates/ruff_linter/src/linter.rs`:1046 on 2025-12-02 11:04_

done

---

_Review requested from @ntBre by @amyreese on 2025-12-02 23:14_

---

_@ntBre reviewed on 2025-12-02 23:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:1046 on 2025-12-02 23:38_

Oh I see, the error doesn't trigger at all in `test_semantic_errors` because we map it back to the rule. I will restore the `test_syntax_errors` version that you had initially in that case and just clarify the `ok` comments.

I think it's still nice to have the B901 tests you added :+1:

---

_@ntBre reviewed on 2025-12-02 23:43_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1727 on 2025-12-02 23:43_

After this change, we didn't need the `Stmt::Expr` part anymore. I also copied this change over to B901 proper.

For future reference, I also checked the blame for the "not broken" B901 tests that this changed. They were part of the original PR adding the rule (https://github.com/astral-sh/ruff/pull/11644) and were copied from the upstream linter. However, I think the upstream tests were added incorrectly in https://github.com/PyCQA/flake8-bugbear/commit/05002b5cc7b56f06ff2f84a133339eedb0f470f2 when the real fix was to avoid flagging the error in `__await__` methods, as we already do. As I noted in one of my commit messages, these cases _are_ syntax errors in async generators, so I think we should be flagging them.

---

_@ntBre approved on 2025-12-02 23:43_

Thanks! I pushed a few commits. Just waiting for CI and then I'll merge!

---

_Comment by @ntBre on 2025-12-03 00:08_

Hmm, there are actually 35 new ecosystem hits. They all look like true positives to me, and this is a preview rule, so I think it's okay to accept this, but @amyreese, do you know if code like this is common enough that we should allow it?

```py
@pytest.hookimpl(wrapper=True, tryfirst=True)
def pytest_collection(session: Session) -> Generator[None, object, object]:
    config = session.config
    with catch_warnings_for_item(
        config=config, ihook=config.hook, when="collect", item=None
    ):
        return (yield)  # <--
```

It popped up many times in `pytest`, but maybe they just wouldn't want to enable this rule.

It seems to me that the motivation for the rule applies whether the yield is a standalone expression or not. But we should probably include this in the release notes, either by splitting out the visitor change/bug fix or at least by updating the title and labels.

For a bit more context, my suggestion to start flagging these was in https://github.com/astral-sh/ruff/pull/21200#discussion_r2578909298.

---

_Comment by @amyreese on 2025-12-03 01:37_

I've literally never seen `return (yield)` in the wild before today. 😅

Since most of the ecosystem hits seem related to pytest hooks, I think it would be ok as-is — it's a really weird pattern and seems to negate most of the "purpose" of a generator function. 

I presume that pytest is doing something like sending a value back into a generator and then capturing that in the `StopIteration` when it returns, and I suspect they would already have B901 disabled.

---

_Comment by @amyreese on 2025-12-03 01:41_

2.2k hits on github, some of which at least seem mildly more useful than what I saw in pytest: https://github.com/search?q=%22return+%28yield%29%22&type=code

I can't imagine people are actually using the actual value from their yield expressions as a return value, probably more as shorthand for `yield X; return`.

I guess as exceptions to a rule go, it's not crazy to allow it, just weird.

---

_Comment by @ntBre on 2025-12-03 15:33_

Thanks! Sounds like it's probably reasonable to land this, especially since the rule is still in preview. I'll try to update the title and labels so that it shows up in the release notes.

---

_Renamed from "[syntax-errors]: semantic syntax error for async generator with return" to "[`flake8-bugbear`] Catch `yield` expressions within other statements (`B901`)" by @ntBre on 2025-12-03 15:34_

---

_Label `internal` removed by @ntBre on 2025-12-03 15:42_

---

_Label `rule` added by @ntBre on 2025-12-03 15:42_

---

_Label `preview` added by @ntBre on 2025-12-03 15:42_

---

_Comment by @codspeed-hq[bot] on 2025-12-03 15:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/11happy%3Areturn_in_async_generator?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21200 will **not alter performance**

<sub>Comparing <code>11happy:return_in_async_generator</code> (fe9fb9a) with <code>main</code> (4488e9d)</sub>



### Summary

`✅ 52` untouched  





---

_Comment by @ntBre on 2025-12-03 15:54_

Hmm, these benchmarks don't seem right. I'll try to merge main again.

---

_Comment by @AlexWaygood on 2025-12-03 16:09_

> Hmm, these benchmarks don't seem right. I'll try to merge main again.

The pydantic benchmark seems to fairly regularly vary around 5% or so since we landed https://github.com/astral-sh/ruff/pull/21467, unfortunately :/

---

_Merged by @ntBre on 2025-12-03 17:05_

---

_Closed by @ntBre on 2025-12-03 17:05_

---

_Comment by @ntBre on 2025-12-03 21:54_

Hmm maybe I should have searched around a bit more. I stumbled across https://github.com/astral-sh/ruff/issues/14453 and https://github.com/astral-sh/ruff/pull/15101 and https://github.com/astral-sh/ruff/pull/15254 for unrelated reasons this afternoon. Apparently this change has already been considered and rejected.

I still think it makes sense to flag these, including after checking the upstream implementation (https://github.com/astral-sh/ruff/pull/21200#discussion_r2583138174), but I guess we should either revert the visitor changes (not the syntax error part) like #15254 or close #14453 as now completed. As I say in my other comment, we at least have to visit sub-expressions in the async case because that's a syntax error:

```pycon
>>> async def foo():
...     return (yield 1)
...
  File "<python-input-0>", line 2
    return (yield 1)
    ^^^^^^^^^^^^^^^^
SyntaxError: 'return' with value in async generator
```

but I think that bolsters the sync case too.

cc @MichaReiser who reviewed the earlier PR

---
