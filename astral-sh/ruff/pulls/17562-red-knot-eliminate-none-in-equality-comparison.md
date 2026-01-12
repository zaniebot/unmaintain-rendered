```yaml
number: 17562
title: "[red-knot] Eliminate `None` in equality-comparison narrowing"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/eq-narrowing-none
created_at: 2025-04-22T19:47:38Z
updated_at: 2025-04-23T06:45:17Z
url: https://github.com/astral-sh/ruff/pull/17562
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Eliminate `None` in equality-comparison narrowing

---

_@sharkdp_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @sharkdp on 2025-04-22 19:47_

---

_Comment by @github-actions[bot] on 2025-04-22 19:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dragonchain (https://github.com/dragonchain/dragonchain)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dragonchain/dragonchain/lib/dto/eth.py:98:13: Operator `-=` is unsupported between objects of type `None` and `Literal[60]`
- Found 402 diagnostics
+ Found 401 diagnostics

rotki (https://github.com/rotki/rotki)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/tests/db/test_db.py:1512:12: Attribute `serialize_key` on type `PremiumCredentials | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rotki/rotkehlchen/tests/db/test_db.py:1513:12: Attribute `serialize_secret` on type `PremiumCredentials | None` is possibly unbound
- Found 2163 diagnostics
+ Found 2161 diagnostics

zulip (https://github.com/zulip/zulip)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zulip/tools/lib/pretty_print.py:93:32: Type `None` has no attribute `indent`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zulip/tools/lib/pretty_print.py:94:27: Type `None` has no attribute `parent_token`
- Found 4441 diagnostics
+ Found 4439 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_iast/taint_sinks/ast_taint.py:40:54: Operator `+` is unsupported between objects of type `Literal["."] | @Todo(Intersection meta-type)` and `Any | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_iast/taint_sinks/ast_taint.py:40:54: Operator `+` is unsupported between objects of type `Literal["."] | @Todo(map_with_boundness: intersections with negative contributions)` and `Any | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_atr.py:97:47: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_atr.py:187:39: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_efd.py:80:42: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_efd.py:81:43: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_efd.py:167:43: Argument to this function is incorrect: Expected `int`, found `int | None`
- Found 6927 diagnostics
+ Found 6922 diagnostics

```
</details>


---

_Closed by @sharkdp on 2025-04-23 06:45_

---
