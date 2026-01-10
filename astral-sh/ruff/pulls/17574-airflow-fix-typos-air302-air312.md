```yaml
number: 17574
title: "[`airflow`] fix typos (`AIR302`, `AIR312`)"
type: pull_request
state: merged
author: camper42
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-airflow-help-msg
created_at: 2025-04-23T07:36:10Z
updated_at: 2025-04-24T06:06:32Z
url: https://github.com/astral-sh/ruff/pull/17574
synced_at: 2026-01-10T19:33:02Z
```

# [`airflow`] fix typos (`AIR302`, `AIR312`)

---

_Pull request opened by @camper42 on 2025-04-23 07:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix Airflow Provider package name

`apache-airflow-provider-{provider}` -> `apache-airflow-providers-{provider}` (`provider` -> `providers`)

with ruff 0.11.6, package name in help msg is not correct, e.g.:

```
   = help: Install `apache-airflow-provider-standard>=0.0.1` and use `airflow.providers.standard.operators.python.ShortCircuitOperator` instead.
```

when running `uv add apache-airflow-provider-standard`, I get:

```
❯ uv add apache-airflow-provider-standard
  × No solution found when resolving dependencies for split (python_full_version >= '3.13'):
  ╰─▶ Because apache-airflow-provider-standard was not found in the package registry and your project depends on apache-airflow-provider-standard, we can conclude that your project's requirements
      are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

correct package name is `apache-airflow-providers-standard`

ref: https://airflow.apache.org/docs/apache-airflow-providers-standard/stable/index.html

## Test Plan

The test fixture has been updated accordingly

---

_@ntBre approved on 2025-04-23 14:17_

Thanks! @Lee-W does this look good to you?

---

_Label `bug` added by @ntBre on 2025-04-23 14:17_

---

_Comment by @github-actions[bot] on 2025-04-23 14:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Lee-W on 2025-04-24 00:07_

Yep, thanks for pointing out!

---

_Merged by @MichaReiser on 2025-04-24 06:06_

---

_Closed by @MichaReiser on 2025-04-24 06:06_

---
