```yaml
number: 6669
title: "Make `Parameters` an optional field on `ExprLambda`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/lambda
created_at: 2023-08-18T04:36:07Z
updated_at: 2023-08-18T15:34:55Z
url: https://github.com/astral-sh/ruff/pull/6669
synced_at: 2026-01-12T15:55:22Z
```

# Make `Parameters` an optional field on `ExprLambda`

---

_@charliermarsh_

## Summary

If a lambda doesn't contain any parameters, or any parameter _tokens_ (like `*`), we can use `None` for the parameters. This feels like a better representation to me, since, e.g., what should the `TextRange` be for a non-existent set of parameters? It also allows us to remove several sites where we check if the `Parameters` is empty by seeing if it contains any arguments, so semantically, we're already trying to detect and model around this elsewhere.

Changing this also fixes a number of issues with dangling comments in parameter-less lambdas, since those comments are now automatically marked as dangling on the lambda. (As-is, we were also doing something not-great whereby the lambda was responsible for formatting dangling comments on the parameters, which has been removed.)

Closes https://github.com/astral-sh/ruff/issues/6646.

Closes https://github.com/astral-sh/ruff/issues/6647.

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2023-08-18 04:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pie/rules/reimplemented_list_builtin.rs`:62 on 2023-08-18 04:36_

Much better IMO.

---

_@charliermarsh reviewed on 2023-08-18 04:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_codegen/src/generator.rs`:881 on 2023-08-18 04:36_

Also much better.

---

_@charliermarsh reviewed on 2023-08-18 04:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:245 on 2023-08-18 04:37_

If the lambda does have parameters, and those parameters have dangling comments, they're now handled by the lambda directly.

---

_@charliermarsh reviewed on 2023-08-18 04:38_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:245 on 2023-08-18 04:38_

If the lambda doesn't have parameters, and itself has dangling comments, those are formatted automatically by the lambda as "standard" dangling comments.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:167 on 2023-08-18 04:48_

Calling `handle_bracketed_end_of_line_comment` here for lambda parameters (which are never parenthesized) triggered a debug assertion in that method. Similar to tuples, we should skip this check for unparenthesized cases.

---

_@charliermarsh reviewed on 2023-08-18 04:48_

---

_Label `formatter` added by @charliermarsh on 2023-08-18 04:48_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-18 04:49_

---

_@charliermarsh reviewed on 2023-08-18 04:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:877 on 2023-08-18 04:49_

All the changes in `crates/ruff` are fairly mechanical -- just reacting to the fact that this field is now optional.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:252 on 2023-08-18 05:45_

Would it make sense to only store existing parameters? Or is it important to know how many lambdas have a `None` parameter?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:245 on 2023-08-18 05:49_

Formatting dangling comments of another node can be problematic if we e.g. want to support suppression of any node, because it is impossible to *undo* the formatting that was done by the parent. 

Is there another way to get the desired formatting that leaves the dangling comment formatting inside of `Parameters`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:52 on 2023-08-18 05:50_

<i class='graphite__hidden'>[Re: lines 46 to 46]<br/></i>

This comment is no longer true.<span class='graphite__hidden'><br/><br/>See this comment inline on <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6669?utm_source=unchanged-line-comment">Graphite</a>.</span>



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:685 on 2023-08-18 05:52_

Nit: Unrelated to your PR. It would be nice if `slice` accepts `Into<Ranged>` but that probably requires moving `Ranged` to `ruff_text_size`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:685 on 2023-08-18 05:52_

Nit: I find `Locator` for slicing a bit overkill: 

```rust
&source[parameters.range].starts_with('(')
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:683 on 2023-08-18 05:53_

Where is this used? Should it be moved closer to the file that is using it?

---

_@MichaReiser approved on 2023-08-18 05:55_

Looks good. 

The only thing that would be good is to avoid formatting dangling comments outside of Parameters. It breaks the concept of that a node formats the content inside of its range. There's no consequence of doing this here but e.g. doing this at the statement level breaks suppression comments and may set a bad precedence. 

---

_@charliermarsh reviewed on 2023-08-18 13:41_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:683 on 2023-08-18 13:41_

It's used in `placement.rs`. I was torn.

---

_@charliermarsh reviewed on 2023-08-18 13:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:685 on 2023-08-18 13:42_

I might do this, I think `Ranged` _should_ be in `ruff_text_size`.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:245 on 2023-08-18 13:43_

Hmm, but the point is that the parent only handles dangling comments if the parameters don't exist at all (are `None`).

---

_@charliermarsh reviewed on 2023-08-18 13:43_

---

_@charliermarsh reviewed on 2023-08-18 13:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:245 on 2023-08-18 13:43_

In other words, there's no longer any formatting of dangling comments on another node (whereas there was before).

---

_@charliermarsh reviewed on 2023-08-18 15:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameters.rs`:685 on 2023-08-18 15:01_

@zanieb probably gonna do this per offline conversation.

---

_@MichaReiser reviewed on 2023-08-18 15:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:52 on 2023-08-18 15:04_

Hmm, it's pretty annoying that you have to go to graphite to see the commented lines. 

---

_@charliermarsh reviewed on 2023-08-18 15:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:252 on 2023-08-18 15:12_

We need to know if there are _any_ lambdas, i.e., whether this vector is non-empty.

---

_Merged by @charliermarsh on 2023-08-18 15:34_

---

_Closed by @charliermarsh on 2023-08-18 15:34_

---

_Branch deleted on 2023-08-18 15:34_

---
