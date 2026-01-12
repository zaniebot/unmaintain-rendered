```yaml
number: 7468
title: "Respect parentheses for precedence in `await`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/await
created_at: 2023-09-17T18:41:30Z
updated_at: 2023-09-18T14:02:02Z
url: https://github.com/astral-sh/ruff/pull/7468
synced_at: 2026-01-12T02:39:10Z
```

# Respect parentheses for precedence in `await`

---

_Pull request opened by @charliermarsh on 2023-09-17 18:41_

## Summary

We were using `Parenthesize::IfBreaks` universally for `await`, but dropping parentheses can change the AST due to precedence. It turns out that Black's rules aren't _exactly_ the same as operator precedence (e.g., they leave parentheses around `await ([1, 2, 3])`, although they aren't strictly required).

Closes https://github.com/astral-sh/ruff/issues/7467.

## Test Plan

`cargo test`

No change in similarity.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               398 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99962 |              1437 |                22 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99982 |              2760 |                37 |
| transformers |           0.99957 |              2587 |               398 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99929 |               648 |                16 |
| zulip        |           0.99962 |              1437 |                22 |


---

_Label `bug` added by @charliermarsh on 2023-09-17 18:41_

---

_Label `formatter` added by @charliermarsh on 2023-09-17 18:41_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-17 18:46_

---

_Comment by @codspeed-hq[bot] on 2023-09-17 18:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/await)

### Merging #7468 will **degrade performances by 4.6%**

<sub>Comparing <code>charlie/await</code> (165b7dd) with <code>main</code> (c4d85d6)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/await)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/await` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `formatter[large/dataset.py]` | 58.9 ms | 61.8 ms | -4.6% |


---

_Comment by @MichaReiser on 2023-09-17 19:08_

The similarity numbers look incorrect. Django should only have like 20 files with changes 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_await.rs`:51 on 2023-09-17 19:09_

Why Optional and not always if they're required to maintain the same precedence?

---

_Comment by @charliermarsh on 2023-09-17 19:10_

They are copied exactly from the CI output here and on main.

---

_Comment by @charliermarsh on 2023-09-17 19:11_

It looks like it changed at some point (although the similarity score did not), I'm bisecting.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_await.rs`:22 on 2023-09-17 19:11_

I see how this is somewhat easier to implement but wonder it it should instead be moved into NeedsParentheses of the individual expressions to better align with the overall design 

---

_@MichaReiser reviewed on 2023-09-17 19:11_

---

_Comment by @MichaReiser on 2023-09-17 19:13_

> They are copied exactly from the CI output here and on main.

My preferred way to get this numbers is to just run the script locally 

---

_Comment by @charliermarsh on 2023-09-17 19:13_

@MichaReiser - It looks like the number changed with your most recent PR (https://github.com/astral-sh/ruff/actions/runs/6214254837/job/16866021385). The preceding commit shows 37 changed files (https://github.com/astral-sh/ruff/actions/runs/6209449877/job/16856667858).

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:51 on 2023-09-17 19:15_

Only some of these "always" need parentheses (like boolean operators). For some of these (like lists, tuples, etc.), we only want to preserve parentheses if present. I could separate the two cases if preferred.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:22 on 2023-09-17 19:18_

Hmm... I'm torn? This feels so much easier to understand since it's in a centralized place that's coupled to the `await` formatting, but I can understand why moving this into the individual expressions adheres more closely to the overall design.

---

_@charliermarsh reviewed on 2023-09-17 19:18_

---

_Comment by @charliermarsh on 2023-09-17 19:18_

> My preferred way to get this numbers is to just run the script locally

Are you saying this is what you prefer to do, or what you consider to be a better practice? :)

---

_@charliermarsh reviewed on 2023-09-17 20:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:22 on 2023-09-17 20:13_

On the other hand, this feels more consistent with how (e.g.) `expr_yield` works? We always preserve parentheses (`Parenthesize::Optional`), and so don't require that expressions encode yield precedence in their own `NeedsParentheses`.

---

_@MichaReiser reviewed on 2023-09-17 20:26_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_await.rs`:22 on 2023-09-17 20:26_

> On the other hand, this feels more consistent with how (e.g.) `expr_yield` works? We always preserve parentheses (`Parenthesize::Optional`), and so don't require that expressions encode yield precedence in their own `NeedsParentheses`.

Isn't that different because `yield` always preserve parentheses for all kind of expressions whereas `await` only preserves for some (which is what `NeedsParentheses` is for).

---

_@charliermarsh reviewed on 2023-09-17 20:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:22 on 2023-09-17 20:30_

Yes but isn't it strange that the `NeedsParentheses` for (e.g.) boolean operators would need to take into account whether we always preserve parentheses for `yield` vs. only sometimes do so for `await`? Parentheses are required in both cases, but the suggestion here is that we add a condition for `await` to the `NeedsParentheses` implementation.

(Perhaps the "correct" solution would be to include both `await` and `yield` and whatever else in that `NeedsParentheses` implementation, even though it would be redundant for `yield` in practice due to the fact that we preserve parentheses in the parent.)

---

_@dhruvmanila reviewed on 2023-09-18 04:27_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/expr_await.rs`:51 on 2023-09-18 04:27_

To give some context around `IpyEscapeCommand`, they should never be parenthesized and can only be present on the right side of an assignment statement.

---

_@konstin approved on 2023-09-18 07:45_

---

_@MichaReiser reviewed on 2023-09-18 11:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_await.rs`:22 on 2023-09-18 11:48_

Maybe `Yield` should be changed too ;) Altough I think `yield` is different because the parentheses are always preserves (as far as I understand). 

This is different from `await` where the use is a "hack" to get the `Always` because it simply relies on the fact that the parentheses are already there. This is also slightly slower because the formatter has to test whether the expression is parenthesized in the source. 

We could include `yield` in `NeedsParentheses` but it ends up being untested code. 

---

_@MichaReiser approved on 2023-09-18 11:48_

Still leaning towards moving this inside `NeedsParantheses` for consistency. 

---

_@charliermarsh reviewed on 2023-09-18 13:45_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_await.rs`:22 on 2023-09-18 13:45_

Changed the various `NeedsParentheses`.

---

_Merged by @charliermarsh on 2023-09-18 13:56_

---

_Closed by @charliermarsh on 2023-09-18 13:56_

---

_Branch deleted on 2023-09-18 13:56_

---
