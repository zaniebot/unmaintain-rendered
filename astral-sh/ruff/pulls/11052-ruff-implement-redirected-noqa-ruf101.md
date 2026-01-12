```yaml
number: 11052
title: "[`ruff`] Implement `redirected-noqa` (`RUF101`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: ruf101
created_at: 2024-04-20T02:06:12Z
updated_at: 2024-04-27T03:22:40Z
url: https://github.com/astral-sh/ruff/pull/11052
synced_at: 2026-01-12T15:55:34Z
```

# [`ruff`] Implement `redirected-noqa` (`RUF101`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Based on discussion in #10850.

As it stands today `RUF100` will attempt to replace code redirects with their target codes even though this is not the "goal" of `RUF100`. This behavior is confusing and inconsistent, since code redirects which don't otherwise violate `RUF100` will not be updated. The behavior is also undocumented. Additionally, users who want to use `RUF100` but do not want to update redirects have no way to opt out.

This PR explicitly detects redirects with a new rule `RUF101` and patches `RUF100` to keep original codes in fixes and reporting.

## Test Plan

Added fixture.

---

_Comment by @github-actions[bot] on 2024-04-20 02:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:64:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `B035`)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:64:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `RUF011`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -1 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/disnake/utils.py#L1161'>disnake/utils.py:1161:56:</a> RUF101 [*] `PGH001` is a redirect to `S307`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:64:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `B035`)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:64:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `RUF011`)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/graph_components/validators/test_default_recipe_validator.py#L815'>tests/graph_components/validators/test_default_recipe_validator.py:815:72:</a> RUF101 [*] `RUF011` is a redirect to `B035`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF101 | 2 | 2 | 0 | 0 | 0 |
| RUF100 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-04-26 22:44_

Sorry, I know this is blocked on my review.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-26 22:44_

---

_Label `rule` added by @charliermarsh on 2024-04-26 22:44_

---

_@charliermarsh approved on 2024-04-27 01:53_

Thanks!

---

_Label `preview` added by @charliermarsh on 2024-04-27 02:01_

---

_Merged by @charliermarsh on 2024-04-27 02:08_

---

_Closed by @charliermarsh on 2024-04-27 02:08_

---

_Branch deleted on 2024-04-27 03:21_

---

_Comment by @augustelalande on 2024-04-27 03:22_

Thanks for taking the time 😄 

---
