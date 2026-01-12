```yaml
number: 6881
title: "Introduce AST nodes for `PatternMatchClass` arguments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pattern-ast
created_at: 2023-08-25T20:20:58Z
updated_at: 2023-08-26T14:50:34Z
url: https://github.com/astral-sh/ruff/pull/6881
synced_at: 2026-01-12T15:55:22Z
```

# Introduce AST nodes for `PatternMatchClass` arguments

---

_@charliermarsh_

## Summary

This PR introduces two new AST nodes to improve the representation of `PatternMatchClass`. As a reminder, `PatternMatchClass` looks like this:

```python
case Point2D(0, 0, x=1, y=2):
  ...
```

Historically, this was represented as a vector of patterns (for the `0, 0` portion) and parallel vectors of keyword names (for `x` and `y`) and values (for `1` and `2`). This introduces a bunch of challenges for the formatter, but importantly, it's also really different from how we represent similar nodes, like arguments (`func(0, 0, x=1, y=2)`) or parameters (`def func(x, y)`).

So, firstly, we now use a single node (`PatternArguments`) for the entire parenthesized region, making it much more consistent with our other nodes. So, above, `PatternArguments` would be `(0, 0, x=1, y=2)`.

Secondly, we now have a `PatternKeyword` node for `x=1` and `y=2`. This is much more similar to the how `Keyword` is represented within `Arguments` for call expressions.

Closes https://github.com/astral-sh/ruff/issues/6866.

Closes https://github.com/astral-sh/ruff/issues/6880.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-25 20:21_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1304 on 2023-08-25 20:21_

This gets dramatically simplified.

---

_@charliermarsh reviewed on 2023-08-25 20:21_

---

_@charliermarsh reviewed on 2023-08-25 20:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/pattern/pattern_arguments.rs`:12 on 2023-08-25 20:24_

This is very, very similar to `arguments.rs`, but it's difficult to share much code.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/pattern/pattern_match_class.rs`:126 on 2023-08-25 20:24_

This specific logic is way simpler now -- we only care if there are dangling comments, not where they are, since these are dangling comments between `Point2D` and its arguments.

---

_@charliermarsh reviewed on 2023-08-25 20:24_

---

_Comment by @charliermarsh on 2023-08-25 20:25_

Adding these AST nodes does require a bunch of boilerplate, but I think it's worth it for the improved representation.

---

_Label `internal` added by @charliermarsh on 2023-08-25 20:30_

---

_Comment by @github-actions[bot] on 2023-08-25 20:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.lalrpop`:818 on 2023-08-26 10:26_

Nit: I got really confused by `ClassArguments`. I first thought you have two rules, one for `ClassArguments` and one for `PatternArguments`. I recommend renaming the rule to the same name as the node as we do for other rules too

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1304 on 2023-08-26 10:28_

...dramatically :laughing: 

---

_@MichaReiser approved on 2023-08-26 10:29_

Can you verify that the following continues to parse successfully (maybe add a test?) 

```python
match a:
   case Point2D((0), 0, x=(1), y=2):
        ...
```

---

_@charliermarsh reviewed on 2023-08-26 14:34_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1304 on 2023-08-26 14:34_

Anything that deletes code is a "dramatic simplification" for me

---

_Merged by @charliermarsh on 2023-08-26 14:45_

---

_Closed by @charliermarsh on 2023-08-26 14:45_

---

_Branch deleted on 2023-08-26 14:45_

---

_Comment by @codspeed-hq[bot] on 2023-08-26 14:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/pattern-ast)

### Merging #6881 will create unknown performance changes

<sub>Comparing <code>charlie/pattern-ast</code> (3b31526) with <code>main</code> (ed1b412)</sub>



### Summary


`🆕 16` new benchmarks



### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/pattern-ast` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `formatter[numpy/globals.py]` | N/A | 1.4 ms | N/A |
| 🆕 | `linter/default-rules[large/dataset.py]` | N/A | 91.9 ms | N/A |
| 🆕 | `formatter[large/dataset.py]` | N/A | 72.7 ms | N/A |
| 🆕 | `linter/default-rules[numpy/ctypeslib.py]` | N/A | 18.7 ms | N/A |
| 🆕 | `linter/all-rules[numpy/globals.py]` | N/A | 4 ms | N/A |
| 🆕 | `formatter[numpy/ctypeslib.py]` | N/A | 13.9 ms | N/A |
| 🆕 | `linter/default-rules[pydantic/types.py]` | N/A | 39 ms | N/A |
| 🆕 | `parser[numpy/globals.py]` | N/A | 1.4 ms | N/A |
| 🆕 | `parser[numpy/ctypeslib.py]` | N/A | 12.5 ms | N/A |
| 🆕 | `parser[pydantic/types.py]` | N/A | 26.8 ms | N/A |
| 🆕 | `parser[large/dataset.py]` | N/A | 68.8 ms | N/A |
| 🆕 | `linter/all-rules[pydantic/types.py]` | N/A | 71.8 ms | N/A |
| 🆕 | `linter/default-rules[numpy/globals.py]` | N/A | 2.2 ms | N/A |
| 🆕 | `linter/all-rules[numpy/ctypeslib.py]` | N/A | 34.2 ms | N/A |
| 🆕 | `linter/all-rules[large/dataset.py]` | N/A | 156.1 ms | N/A |
| 🆕 | `formatter[pydantic/types.py]` | N/A | 26.3 ms | N/A |


---
