```yaml
number: 17999
title: Skip S608 for expressionless f-strings
type: pull_request
state: merged
author: maxmynter
labels:
  - rule
assignees: []
merged: true
base: main
head: autofix-s608
created_at: 2025-05-10T04:02:08Z
updated_at: 2025-05-10T10:38:00Z
url: https://github.com/astral-sh/ruff/pull/17999
synced_at: 2026-01-12T15:56:10Z
```

# Skip S608 for expressionless f-strings

---

_@maxmynter_


<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes: #17967
## Summary
Skip `S608` for expressionless f-strings.
Expressionless f-strings are not vulnerable to SQL injections and F541 catches expressionless f-strings with an autofix.

This is achieved with guarding on the `S608` f-string match with an occurrence check for expressions within it; analogous to the implementation of F451. 


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added a test fixture and updated snapshot.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-10 04:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/73701b729563d796d4aae628eff24f7163e7671c/tests/integration_tests/sqllab_tests.py#L191'>tests/integration_tests/sqllab_tests.py:191:58:</a> RUF100 [*] Unused `noqa` directive (unused: `S608`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/73701b729563d796d4aae628eff24f7163e7671c/tests/integration_tests/sqllab_tests.py#L191'>tests/integration_tests/sqllab_tests.py:191:58:</a> RUF100 [*] Unused `noqa` directive (unused: `S608`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @maxmynter on 2025-05-10 06:17_

Ecosystem change is this one: 
```python
names_count = engine.execute(
    f"SELECT COUNT(*) FROM birth_names"  # noqa: F541, S608
).first()
```
https://github.com/apache/superset/blob/73701b729563d796d4aae628eff24f7163e7671c/tests/integration_tests/sqllab_tests.py#L190-L192

It's the intended effect of not raising `S608` when there is no expression in an f-string. The newly raised `RUF100` is marking the the now unused noqa of `S608`.

---

_Label `rule` added by @MichaReiser on 2025-05-10 10:35_

---

_Merged by @MichaReiser on 2025-05-10 10:37_

---

_Closed by @MichaReiser on 2025-05-10 10:37_

---
