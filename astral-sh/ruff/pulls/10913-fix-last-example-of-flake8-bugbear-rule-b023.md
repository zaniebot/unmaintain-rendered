```yaml
number: 10913
title: "Fix last example of flake8-bugbear rule `B023` \"function uses loop variable\""
type: pull_request
state: merged
author: hartwork
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-example-docs-for-B023-function-uses-loop-variable
created_at: 2024-04-12T19:59:44Z
updated_at: 2024-04-12T20:12:42Z
url: https://github.com/astral-sh/ruff/pull/10913
synced_at: 2026-01-12T15:55:33Z
```

# Fix last example of flake8-bugbear rule `B023` "function uses loop variable"

---

_@hartwork_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Hi! :wave: 

Thanks for sharing ruff as software libre — it helps me keep Python code quality up with pre-commit, both locally and in CI :pray: 

While studying the examples at https://docs.astral.sh/ruff/rules/function-uses-loop-variable/#example I noticed that the last of the examples had a bug: prior to this fix, `ì` was passed to the lambda for `x` rather than for `i` — the two are mixed-up. The reason it's easy to overlook is because addition is an commutative operation and so `x + i` and `i + x` give the same result (and least with integers), despite the mix-up. For proof, let me demo the relevant part with before and after:

```python
In [1]: from functools import partial

In [2]: [partial(lambda x, i: (x, i), i)(123) for i in range(3)]
Out[2]: [(0, 123), (1, 123), (2, 123)]

In [3]: [partial(lambda x, i: (x, i), i=i)(123) for i in range(3)]
Out[3]: [(123, 0), (123, 1), (123, 2)]
```

Does that make sense?

## Test Plan

<!-- How was it tested? -->
Was manually tested using IPython.


CC @r4f @grandchild

---

_@charliermarsh approved on 2024-04-12 20:03_

Thanks, this makes sense to me.

---

_Label `documentation` added by @charliermarsh on 2024-04-12 20:03_

---

_Merged by @charliermarsh on 2024-04-12 20:07_

---

_Closed by @charliermarsh on 2024-04-12 20:07_

---

_Comment by @github-actions[bot] on 2024-04-12 20:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
