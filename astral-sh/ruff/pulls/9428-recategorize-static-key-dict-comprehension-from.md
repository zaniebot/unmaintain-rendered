```yaml
number: 9428
title: "Recategorize `static-key-dict-comprehension` from `RUF011` to `B035`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: release/0.2.0
head: charlie/B035
created_at: 2024-01-08T04:09:24Z
updated_at: 2024-01-29T18:00:11Z
url: https://github.com/astral-sh/ruff/pull/9428
synced_at: 2026-01-10T22:57:09Z
```

# Recategorize `static-key-dict-comprehension` from `RUF011` to `B035`

---

_Pull request opened by @charliermarsh on 2024-01-08 04:09_

## Summary

This rule was added to flake8-bugbear. In general, we tend to prefer redirecting to prominent plugins when our own rules are reimplemented (since more projects have `B` activated than `RUF`).

## Test Plan

`cargo test`


---

_Label `rule` added by @charliermarsh on 2024-01-08 04:09_

---

_Renamed from "Redirect `RUF011` to `B035`" to "Recategorize `static-key-dict-comprehension` from `RUF011` to `B035`" by @charliermarsh on 2024-01-08 04:09_

---

_Comment by @MichaReiser on 2024-01-08 07:49_

A few questions here to understand if this is safe to land under our versioning policy or if we need to delay it to the next ~major~ minor release:

* Does this now enable the rule when a user enables the `B` rule group (enabling more rules)
* Is the rule enabled when users use the `RUFF` selector? 
* Do `noqa: RUF011` continue to work?

This is more of a question for our versioning policy: If I remember correctly, this changes the rule codes in diagnostics. We may need to consider this a breaking change because GitLab and other tools that track diagnostics over time now consider the RUF011 diagnostics as fixed but reports new B035 diagnostics. 

---

_Comment by @charliermarsh on 2024-01-08 14:32_

Yeah, we may need to ship this as part of `0.2.0`. Looking for input. To answer your questions:

- Yes.
- No. (It does if they select `RUF011`.)
- Yes.


---

_Comment by @github-actions[bot] on 2024-01-11 02:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:64:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `B035`)
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
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:64:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `B035`)
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

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:21_

---

_Merged by @charliermarsh on 2024-01-29 18:00_

---

_Closed by @charliermarsh on 2024-01-29 18:00_

---

_Branch deleted on 2024-01-29 18:00_

---
