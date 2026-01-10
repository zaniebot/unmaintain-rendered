```yaml
number: 11900
title: "Update `E999` to show all syntax errors"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/syntax-error-1
created_at: 2024-06-17T11:56:51Z
updated_at: 2024-06-19T07:39:55Z
url: https://github.com/astral-sh/ruff/pull/11900
synced_at: 2026-01-10T21:56:00Z
```

# Update `E999` to show all syntax errors

---

_Pull request opened by @dhruvmanila on 2024-06-17 11:56_

## Summary

This PR updates the linter to show all the parse errors as diagnostics instead of just the first one.

Note that this doesn't affect the parse error displayed as error log message. This will be removed in a follow-up PR.

### Breaking?

I don't think this is a breaking change even though this might give more diagnostics. The main reason is that this shouldn't affect any users because it'll only give additional diagnostics in the case of multiple syntax errors.

## Test Plan

Add an integration test case which would raise more than one parse error.

---

_Label `rule` added by @dhruvmanila on 2024-06-18 09:22_

---

_Renamed from "Show all parse errors as diagnostics" to "Update `E999` to show all syntax errors" by @dhruvmanila on 2024-06-18 09:22_

---

_Comment by @github-actions[bot] on 2024-06-18 09:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/99f2a1f53c03232e6421d605bd721f40e5a9370a/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L325'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:325:8:</a> E999 SyntaxError: Multiple exception types must be parenthesized
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/99f2a1f53c03232e6421d605bd721f40e5a9370a/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L325'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:325:8:</a> E999 SyntaxError: Multiple exception types must be parenthesized
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2024-06-18 09:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-18 09:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:204 on 2024-06-18 11:39_

I like the TODO :P

---

_@MichaReiser approved on 2024-06-18 11:39_

---

_@dhruvmanila reviewed on 2024-06-18 12:09_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/linter.rs`:204 on 2024-06-18 12:09_

https://github.com/astral-sh/ruff/pull/11903 ;)

---

_Merged by @dhruvmanila on 2024-06-19 07:39_

---

_Closed by @dhruvmanila on 2024-06-19 07:39_

---

_Branch deleted on 2024-06-19 07:39_

---
