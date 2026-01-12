```yaml
number: 7848
title: Improvements to RUF015
type: pull_request
state: merged
author: hoxbro
labels:
  - bug
assignees: []
merged: true
base: main
head: improve_ruf015
created_at: 2023-10-07T17:15:38Z
updated_at: 2023-10-08T14:55:47Z
url: https://github.com/astral-sh/ruff/pull/7848
synced_at: 2026-01-12T15:55:25Z
```

# Improvements to RUF015

---

_@hoxbro_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ruff/issues/7618. 

The list of builtin iterator is not exhaustive.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

`cargo test`

``` python
a = [1, 2]

examples = [
    enumerate(a),
    filter(lambda x: x, a),
    map(int, a),
    reversed(a),
    zip(a),
    iter(a),
]

for example in examples:
    print(next(example))
```


---

_Comment by @github-actions[bot] on 2023-10-07 17:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-10-08 14:18_

Looks great, made a few small changes!

---

_@charliermarsh reviewed on 2023-10-08 14:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:231 on 2023-10-08 14:19_

I think we can just use the range of the value here. (The previous logic would have failed if there was, e.g., space between the bracket and the star.)

---

_Label `bug` added by @charliermarsh on 2023-10-08 14:20_

---

_@hoxbro reviewed on 2023-10-08 14:46_

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:231 on 2023-10-08 14:46_

I did not think of that... 

I was inspired by the logic done for the `Expr::ListComp` above.  

---

_Merged by @charliermarsh on 2023-10-08 14:49_

---

_Closed by @charliermarsh on 2023-10-08 14:49_

---

_Branch deleted on 2023-10-08 14:51_

---
