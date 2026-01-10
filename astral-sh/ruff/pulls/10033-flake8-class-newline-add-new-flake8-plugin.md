```yaml
number: 10033
title: "[`flake8-class-newline`] Add new flake8 plugin."
type: pull_request
state: closed
author: augustelalande
labels: []
assignees: []
base: main
head: class_newline
created_at: 2024-02-18T23:36:59Z
updated_at: 2024-03-21T18:50:05Z
url: https://github.com/astral-sh/ruff/pull/10033
synced_at: 2026-01-10T22:47:01Z
```

# [`flake8-class-newline`] Add new flake8 plugin.

---

_Pull request opened by @augustelalande on 2024-02-18 23:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add new flake8 plugin, [flake8-class-newline](https://github.com/AlexanderVanEck/flake8-class-newline) which enforces a single blank line between a class definition and its first method.

## Test Plan

Test cases have been added.


---

_Comment by @augustelalande on 2024-02-18 23:55_

I don't really understand the compiling issue, in the test failure.

---

_@MichaReiser reviewed on 2024-02-19 07:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_class_newline/rules/missing_class_newline.rs`:19 on 2024-02-19 07:52_

I think we should exclude `pyi` files from this rule because empty lines in stub files should only be used for grouping methods. 

---

_@augustelalande reviewed on 2024-02-27 03:22_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_class_newline/rules/missing_class_newline.rs`:19 on 2024-02-27 03:22_

Done

---

_Comment by @codspeed-hq[bot] on 2024-02-27 03:26_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande:class_newline)

### Merging #10033 will **degrade performances by 4.04%**

<sub>Comparing <code>augustelalande:class_newline</code> (4282d15) with <code>main</code> (8dc22d5)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/augustelalande:class_newline)._

### Benchmarks breakdown

|     | Benchmark | `main` | `augustelalande:class_newline` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 19.1 ms | 19.9 ms | -4.04% |


---

_@MichaReiser reviewed on 2024-02-27 08:42_

Thanks.

I took another look at the rule and, unfortunately, it is incompatible with our formatter that always removes empty lines between the class header and the first method. 

Ruff doesn't have a good model to support formatter incompatible rules. The problem is that users using `--select ALL` automatically opt in to the rule, and there's a good number of people using `ALL`. We plan to resolve this by [categorizing the rules](https://github.com/astral-sh/ruff/issues/1774). But we can't merge this rule until we figured the categorization out. I'm sorry

---

_Comment by @augustelalande on 2024-02-27 16:24_

Ok no worries, appreciate the time. Should I close this PR, or keep it open for now?

---

_Closed by @augustelalande on 2024-03-21 18:47_

---

_Branch deleted on 2024-03-21 18:50_

---
