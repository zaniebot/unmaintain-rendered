```yaml
number: 5804
title: Format expr generator exp
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-expr-generator-exp
created_at: 2023-07-16T12:53:59Z
updated_at: 2023-07-19T18:42:26Z
url: https://github.com/astral-sh/ruff/pull/5804
synced_at: 2026-01-12T03:30:21Z
```

# Format expr generator exp

---

_Pull request opened by @davidszotten on 2023-07-16 12:53_

## Summary

Format `GeneratorExp`


note: I manually edited `generated.rs` since `FormatExprGeneratorExp` now has a value and needs to be instantiated. it looks like a few others might also be in the same boat (they also break if i re-geneate)

Similarity index for django:  0.978 ->  0.981

Fixes #5676

## Test Plan

snapshots

---

_Comment by @github-actions[bot] on 2023-07-16 13:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.02ms     4.3 MB/sec    1.00      9.4±0.02ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1868.5±1.49µs     8.9 MB/sec    1.01   1882.8±4.23µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    209.3±1.43µs    14.1 MB/sec    1.00    209.4±1.08µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     12.9±0.11ms     3.1 MB/sec    1.00     12.8±0.02ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.5±0.74µs     6.8 MB/sec    1.00    433.8±1.66µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.01ms     6.2 MB/sec    1.00      6.5±0.04ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1414.4±3.20µs    11.8 MB/sec    1.00   1405.3±1.61µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.8±1.35µs    18.8 MB/sec    1.00    155.3±0.28µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.02ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.16ms     3.7 MB/sec    1.13     12.6±0.12ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.7 MB/sec    1.12      2.4±0.04ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00    245.0±5.10µs    12.0 MB/sec    1.09    267.1±9.16µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.20ms     5.3 MB/sec    1.12      5.3±0.08ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.16ms     2.6 MB/sec    1.00     15.7±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    494.0±9.04µs     6.0 MB/sec    1.00    490.1±8.51µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.11ms     3.6 MB/sec    1.00      7.1±0.13ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.00      8.1±0.15ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1690.5±28.26µs     9.8 MB/sec    1.00  1692.8±20.06µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.1±4.22µs    15.4 MB/sec    1.00    192.7±3.60µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.01      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-07-17 10:07_

#5829 fixes the `generate.py`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-17 10:10_

Interesting, that Black has a custom logic especially for generators. 

Can you add an example with a `GeneratorExpr` that also has comments?

```python
len(
	( # leading
	a for b in c
	 # trailing
	)
)
```

Nit: Let's move the `parentheses` check from line 53..59 into the `other => ` branch. We don't need to test if the argument is parenthesized if we're going to remove the parentheses anyway

---

_@MichaReiser approved on 2023-07-17 10:11_

---

_@davidszotten reviewed on 2023-07-17 12:06_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-17 12:06_

this isn't based on black but rather the rules for generator expressions. see e.g. "details, §2b" in the pep https://peps.python.org/pep-0289/#the-details

---

_@davidszotten reviewed on 2023-07-17 12:09_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-17 12:09_

note that your example will differ to black, in my understanding due to the same black bug that keeps _existing parens 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-17 12:25_

> note that your example will differ to black, in my understanding due to the same black bug that keeps _existing parens

Black's behavior is to always preserve parentheses in nested expressions and I think we should do the same for generators, except if we have very good reasons not to (for consistency). 

I think the idiomatic way to implement this then is to change the generator's `NeedsParentheses` implementation to return `Never` if it is the only argument, and `Always` otherwise (except there are other places where the parentheses should be omitted). 

---

_@MichaReiser reviewed on 2023-07-17 12:25_

---

_@davidszotten reviewed on 2023-07-17 12:31_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-17 12:31_

> > note that your example will differ to black, in my understanding due to the same black bug that keeps _existing parens
> 
> Black's behavior is to always preserve parentheses in nested expressions and I think we should do the same for generators, except if we have very good reasons not to (for consistency).
> 

is this a reference to https://github.com/astral-sh/ruff/pull/5804/files#diff-dba753754ca8da36599acf528a6bfe006404be2c4cd9d00f18db1a3cf1a7ec21R9
or https://github.com/astral-sh/ruff/pull/5804/files#diff-dba753754ca8da36599acf528a6bfe006404be2c4cd9d00f18db1a3cf1a7ec21R19
?

> I think the idiomatic way to implement this then is to change the generator's `NeedsParentheses` implementation to return `Never` if it is the only argument, and `Always` otherwise (except there are other places where the parentheses should be omitted).

how does `NeedsParentheses` figure out if it's a no-sibling function argument?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-17 12:36_

> is this a reference to [#5804 (files)](https://github.com/astral-sh/ruff/pull/5804/files#diff-dba753754ca8da36599acf528a6bfe006404be2c4cd9d00f18db1a3cf1a7ec21R9)

Mainly to [#5804 (files)](https://github.com/astral-sh/ruff/pull/5804/files#diff-dba753754ca8da36599acf528a6bfe006404be2c4cd9d00f18db1a3cf1a7ec21R9). Black keeps parentheses around all expressions (and only removes them in specific places like if/for/... headers)

> how does NeedsParentheses figure out if it's a no-sibling function argument?

You get the parent node, which should be an `Arguments` node. You can test if it is the only argument.

---

_@MichaReiser reviewed on 2023-07-17 12:36_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-17 12:40_

> > is this a reference to [#5804 (files)](https://github.com/astral-sh/ruff/pull/5804/files#diff-dba753754ca8da36599acf528a6bfe006404be2c4cd9d00f18db1a3cf1a7ec21R9)
> 
> Mainly to [#5804 (files)](https://github.com/astral-sh/ruff/pull/5804/files#diff-dba753754ca8da36599acf528a6bfe006404be2c4cd9d00f18db1a3cf1a7ec21R9). Black keeps parentheses around all expressions (and only removes them in specific places like if/for/... headers)

oh did i misremember that it was ok to diverge from black if it has known/accepted bug reports? (like the one i link to)

---

_@davidszotten reviewed on 2023-07-17 12:40_

---

_@MichaReiser reviewed on 2023-07-18 09:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-18 09:07_

I think it is okay if it is a functional bug of Black. This seems like a formatting change that has been accepted but will probably not be rolled out before the next black styleguide version.

---

_@davidszotten reviewed on 2023-07-18 09:24_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-18 09:24_

ok. i'm afraid i don't quite understand your fix suggestion. atm `NeedsParentheses` (afict) is only called if there is a `maybe_parenthesize_expression` wrapper. is that right? if so, where are you suggesting that should go? apologies if i'm missing something obvious here

---

_Label `formatter` added by @konstin on 2023-07-18 11:08_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/generator_exp.py`:9 on 2023-07-18 16:34_

```suggestion
# black keeps these atm, but intends to remove them in the future: https://github.com/psf/black/issues/2943
```

---

_@konstin approved on 2023-07-18 16:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-19 06:08_

The proposal with `NeedsParentheses` is mainly if we want to match Black's behavior:

* Because, by default, expressions preserve their parentheses. Meaning the generators would have needed parentheses already before. Or are you aware of situations where a generator is not parenthesized in the source, but would need to be parenthesized after formatting (e.g. because we insert a trailing comma)?
* Overriding `NeedsParentheses` and returning `Always` will add/preserve parentheses in the places where we, otherwise would omit them (e.g. in an if header). 

The reason why I think that `NeedsParentheses` would be a good fit is, because it should inform the formatted when it is necessary to parenthesize an expression. Tuples are different, because they have their own parentheses (you can have a parenthesized tuple expression `((a, b))`, but `(a, b)` is a tuple expression with parentheses. Okay this sounds confusing. One is about it is a parenthesized expression, the other is that tuples have their own parentheses)


---

_@MichaReiser reviewed on 2023-07-19 06:08_

---

_@davidszotten reviewed on 2023-07-19 08:23_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-19 08:23_

maybe i'm misunderstanding you, but in my understanding of generator expressions, they do have their "own parentheses" just like tuples do (but they can be omitted in some contexts. (fewer contexts than for tuples, only if they are a sole call argument))

maybe we merge this without preserving "double wrapped" generators and see how common when we compare ourselves against black on the various code bases we test. in general i'm in favour of matching black closer if that helps adoption

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-19 11:00_

Oh I'm sorry. That's my bad. I assumed that parenthesizing the generator is optional. But from Python's full grammar:

```
genexp:
    | '(' ( assignment_expression | expression !':=') for_if_clauses ')'
```
> https://docs.python.org/3/reference/grammar.html

(Oddly, the parens are not optional)

---

_@MichaReiser reviewed on 2023-07-19 11:00_

---

_@MichaReiser approved on 2023-07-19 11:01_

---

_Merged by @MichaReiser on 2023-07-19 11:01_

---

_Closed by @MichaReiser on 2023-07-19 11:01_

---

_@davidszotten reviewed on 2023-07-19 18:42_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_call.rs`:70 on 2023-07-19 18:42_

i wonder if the function call case is covered here

```
primary:
    | primary '.' NAME 
    | primary genexp                     <--  generator expr instead of brackets and arguments
    | primary '(' [arguments] ')'      <-- (regular function call)
```

---
