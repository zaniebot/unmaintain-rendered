```yaml
number: 7503
title: "feat(rules): implement `flake8-bandit` `S201` (`flask_debug_true`)"
type: pull_request
state: merged
author: mkniewallner
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/add-flake8-bandit-S201
created_at: 2023-09-19T00:03:00Z
updated_at: 2023-09-19T00:53:43Z
url: https://github.com/astral-sh/ruff/pull/7503
synced_at: 2026-01-12T02:39:10Z
```

# feat(rules): implement `flake8-bandit` `S201` (`flask_debug_true`)

---

_Pull request opened by @mkniewallner on 2023-09-19 00:03_

Part of #1646.

## Summary

Implement `S201` ([`flask_debug_true`](https://bandit.readthedocs.io/en/latest/plugins/b201_flask_debug_true.html)) rule from `bandit`.

I am fairly new to Rust and Ruff's codebase, so there might be better ways to implement the rule or write the code.

## Test Plan

Snapshot test from https://github.com/PyCQA/bandit/blob/1.7.5/examples/flask_debug.py, with a few additions in the "unrelated" part to test a bit more cases.

---

_Comment by @codspeed-hq[bot] on 2023-09-19 00:12_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mkniewallner:feat/add-flake8-bandit-S201)

### Merging #7503 will **improve performances by 2.29%**

<sub>Comparing <code>mkniewallner:feat/add-flake8-bandit-S201</code> (7ed0354) with <code>main</code> (40f6456)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `mkniewallner:feat/add-flake8-bandit-S201` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 35 ms | 34.3 ms | +2.29% |


---

_Marked ready for review by @mkniewallner on 2023-09-19 00:17_

---

_Comment by @github-actions[bot] on 2023-09-19 00:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:576 on 2023-09-19 00:19_

We're now in the habit of adding new rules under `RuleGroup::Preview` -- mind changing it here?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/flask_debug_true.rs`:13 on 2023-09-19 00:19_

It's annoying, but we manually format these to fit 80 characters, since they sometimes get printed to the terminal.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/flask_debug_true.rs`:57 on 2023-09-19 00:20_

You can reduce one level of indentation by doing:

```
let Some(debug_argument) = call.arguments.find_keyword("debug") else {
    return;
};

...
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/flask_debug_true.rs`:62 on 2023-09-19 00:20_

Same here: can use `let`-`else` to reduce indentation.

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_bandit/S201.py`:9 on 2023-09-19 00:21_

For whatever reason, the comment we typically use here is `# Errors` and then `# OK` (instead of `#okay`).

---

_@charliermarsh reviewed on 2023-09-19 00:21_

Looks great! Just some minor stuff.

---

_Label `rule` added by @charliermarsh on 2023-09-19 00:21_

---

_@charliermarsh approved on 2023-09-19 00:37_

Thanks!

---

_Merged by @charliermarsh on 2023-09-19 00:43_

---

_Closed by @charliermarsh on 2023-09-19 00:43_

---

_Branch deleted on 2023-09-19 00:53_

---
