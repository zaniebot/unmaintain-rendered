```yaml
number: 15625
title: "[`pyupgrade`] Functional enums (`UP048`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
  - preview
assignees: []
base: main
head: UP048
created_at: 2025-01-21T01:29:01Z
updated_at: 2025-03-11T15:53:54Z
url: https://github.com/astral-sh/ruff/pull/15625
synced_at: 2026-01-12T15:55:51Z
```

# [`pyupgrade`] Functional enums (`UP048`)

---

_@InSyncWithFoo_

## Summary

Resolves #12417.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2025-01-21 01:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1c1c391279acde7a14b58600aa9214900683cb5a/tests/serialization/test_serde.py#L152'>tests/serialization/test_serde.py:152:9:</a> UP048 [*] Enum declared using functional syntax
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/5ca9662c4103bf3bc09cbd8ce9a788c0a0a575f6/src/trio/_ssl.py#L253'>src/trio/_ssl.py:253:1:</a> UP048 [*] Enum declared using functional syntax
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/b89c1ce279b6dd7083a5e3795979683de269190f/testing/test_compat.py#L163'>testing/test_compat.py:163:5:</a> UP048 [*] Enum declared using functional syntax
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP048 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2025-01-21 08:18_

---

_Label `preview` added by @MichaReiser on 2025-01-21 08:18_

---

_Comment by @MichaReiser on 2025-01-21 08:22_

I'm not sure UP is a good fit. Using class—or functional-based enums seems more a matter of taste than whether one is considered better (because it was intentionally designed for it) than the other. 

I think this is also visible in the ecosystem checks, where the function-based notation is more concise than the class one. 

Overall, I think we should hold off with this rule. It seems too opinionated. 

---

_Label `needs-decision` added by @MichaReiser on 2025-01-21 08:22_

---

_Closed by @MichaReiser on 2025-03-11 08:48_

---

_Branch deleted on 2025-03-11 15:53_

---
