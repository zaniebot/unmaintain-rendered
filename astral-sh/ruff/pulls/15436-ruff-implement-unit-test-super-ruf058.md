```yaml
number: 15436
title: "[ruff] implement unit-test-super (RUF058)"
type: pull_request
state: closed
author: njhearp
labels:
  - rule
  - needs-decision
  - preview
assignees: []
base: main
head: unit-test-super
created_at: 2025-01-11T19:35:50Z
updated_at: 2025-06-13T18:53:09Z
url: https://github.com/astral-sh/ruff/pull/15436
synced_at: 2026-01-10T18:45:04Z
```

# [ruff] implement unit-test-super (RUF058)

---

_Pull request opened by @njhearp on 2025-01-11 19:35_

## Summary

This implements a rule to catch unit test methods (setUp, tearDown, setUpClass, tearDownClass) that do not call their corresponding super methods. Addresses #14009

## Test Plan

cargo test


---

_Comment by @github-actions[bot] on 2025-01-11 19:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7e57477dea0b2736bbc04466c6b90cb5d6567d23/tools/tests/test_zulint_custom_rules.py#L17'>tools/tests/test_zulint_custom_rules.py:17:9:</a> RUF058 Call super methods for unit test methods
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/477e9283aabc93f8d3ad34ff0b98f68b5a432aa8/testing/example_scripts/unittest/test_setup_skip.py#L10'>testing/example_scripts/unittest/test_setup_skip.py:10:9:</a> RUF058 Call super methods for unit test methods
+ <a href='https://github.com/pytest-dev/pytest/blob/477e9283aabc93f8d3ad34ff0b98f68b5a432aa8/testing/example_scripts/unittest/test_setup_skip_class.py#L11'>testing/example_scripts/unittest/test_setup_skip_class.py:11:9:</a> RUF058 Call super methods for unit test methods
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF058 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unit_test_super.rs`:91 on 2025-01-11 22:27_

Is it intentional to disallow the super call in a nested block: E.g. this would not detect:

```
class Test(...):
	def setUp(self): 
		with SomeContext():
			super.setUp(0
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unit_test_super.rs`:66 on 2025-01-11 22:28_

Nit: I'd use a `match` statement here
```suggestion
            if matches!(name, "setUp" | "tearDown" | "setUpClass" | "tearDownClass) && !has_super_call(body, name) {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unit_test_super.rs`:79 on 2025-01-11 22:28_

We should use the `checker.semantic_model().resolve_qualified_name` to ensure this name resolves to `unittest.TestClass` and not any other class named `TestClass`

---

_@MichaReiser reviewed on 2025-01-11 22:29_

Thanks. I left a few comments. the ecosystem check shows that this rule should be skipped for stub files. 

---

_Label `rule` added by @MichaReiser on 2025-01-11 22:29_

---

_Label `preview` added by @MichaReiser on 2025-01-11 22:29_

---

_@njhearp reviewed on 2025-01-11 22:44_

---

_Review comment by @njhearp on `crates/ruff_linter/src/rules/ruff/rules/unit_test_super.rs`:91 on 2025-01-11 22:44_

That's not intended I'm going to work on a fix.

---

_@njhearp reviewed on 2025-01-11 22:45_

---

_Review comment by @njhearp on `crates/ruff_linter/src/rules/ruff/rules/unit_test_super.rs`:79 on 2025-01-11 22:45_

I'll work on adjusting it to use `checker.semantic_model().resolve_qualified_name`.

---

_Label `needs-decision` added by @MichaReiser on 2025-01-13 07:53_

---

_Comment by @MichaReiser on 2025-01-13 07:54_

We should also come to a conclusion first whether this rule should flag subclasses of `TestCase` or only cases where a class extends from a custom `TestCase`, see https://github.com/astral-sh/ruff/issues/14009

---

_Comment by @njhearp on 2025-01-13 15:23_

I think it would be best to wait for the multi-file analysis. I do not think it would be worth the false negatives as AlexWaygood was mentioning in #14009.

---

_Closed by @njhearp on 2025-01-13 15:23_

---

_Branch deleted on 2025-06-13 18:53_

---
