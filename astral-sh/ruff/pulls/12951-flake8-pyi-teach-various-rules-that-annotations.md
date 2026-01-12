```yaml
number: 12951
title: "[`flake8-pyi`] Teach various rules that annotations might be stringized"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
  - linter
assignees: []
merged: true
base: main
head: pyi034-strings
created_at: 2024-08-17T14:11:53Z
updated_at: 2024-09-02T13:49:24Z
url: https://github.com/astral-sh/ruff/pull/12951
synced_at: 2026-01-12T15:55:42Z
```

# [`flake8-pyi`] Teach various rules that annotations might be stringized

---

_@AlexWaygood_

## Summary

Fixes #12928.

Most `flake8-pyi` rules don't currently consider the possibility that annotations can be stringized -- `foo: "typing.Any"` means the same thing to a type checker as `foo: typing.Any`. This PR adds a general-purpose helper to `ruff_linter/src/rules/flake8_pyi/rules/helpers.rs`, and uses it in several `flake8-pyi` rules to reduce false positives and false negatives on stringized annotations.

I _haven't_ attempted to exhaustively account for stringized annotations in every `flake8-pyi` rule. In some rules, this would have significantly complicated the logic, for marginal gain.

## Test Plan

Several new fixtures and snapshots added.


---

_Label `linter` added by @AlexWaygood on 2024-08-17 14:11_

---

_Comment by @github-actions[bot] on 2024-08-17 14:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/models.py#L330'>securedrop/models.py:330:29:</a> PYI032 [*] Prefer `object` to `Any` for the second parameter to `__eq__`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI032 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/dbdccc593e1aeab149dbd6019709394ec1e6dc9c/securedrop/models.py#L330'>securedrop/models.py:330:29:</a> PYI032 [*] Prefer `object` to `Any` for the second parameter to `__eq__`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI032 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-19 09:13_

---

_Label `bug` added by @MichaReiser on 2024-08-19 09:13_

---

_Label `rule` added by @MichaReiser on 2024-08-19 09:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:82 on 2024-08-19 09:21_

The downside of `match_maybe_stringized_annotation` is that we now end up re-parsing the same string-type annotation for every rule, which is somewhat expensive. 

We could instead:

* Make it a method on `Checker` or `SemanticModel` so that we could add caching if we wanted to (a simple cache could use a single slot)
* We could integrate the rule into `visit_deferred_string_type_definitions` and re-use the parsed type annotation. However, that would be a more invasive change 

---

_@MichaReiser reviewed on 2024-08-19 09:22_

I leave the final review to @charliermarsh who's more familiar with that aspect of ruff

---

_@AlexWaygood reviewed on 2024-08-19 16:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:82 on 2024-08-19 16:00_

> * We could integrate the rule into `visit_deferred_string_type_definitions`

I think that would be tricky because most of these `flake8-pyi` rules need to look at the surrounding function as a whole, rather than just a single annotation inside the function

---

_@AlexWaygood reviewed on 2024-08-29 11:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:82 on 2024-08-29 11:22_

I centralized some of the logic into a method on `Checker` in https://github.com/astral-sh/ruff/pull/12951/commits/87a0c9c25b2dba76370e0139dc3fefa0d1c07aad so that it would be easy to add caching in the future

---

_@charliermarsh approved on 2024-08-29 15:52_

Looks good though I have roughly the same concern as Micha. Caching this would make sense to me. (We already parse these once per file anyway.)

---

_Comment by @AlexWaygood on 2024-08-30 10:25_

> Looks good though I have roughly the same concern as Micha. Caching this would make sense to me.

I separated this out into a standalone PR: #13158

---

_@MichaReiser approved on 2024-09-02 10:22_

---

_Merged by @AlexWaygood on 2024-09-02 13:40_

---

_Closed by @AlexWaygood on 2024-09-02 13:40_

---

_Branch deleted on 2024-09-02 13:40_

---
