```yaml
number: 9384
title: "[`flake8-bandit`] Add `S504` `SslWithNoVersion` rule"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule/S504
created_at: 2024-01-03T20:17:08Z
updated_at: 2024-01-03T23:42:32Z
url: https://github.com/astral-sh/ruff/pull/9384
synced_at: 2026-01-10T23:07:18Z
```

# [`flake8-bandit`] Add `S504` `SslWithNoVersion` rule

---

_Pull request opened by @qdegraaf on 2024-01-03 20:17_

## Summary
Adds `S504` rule for the [flake8-bandit](https://github.com/tylerwince/flake8-bandit) plugin port. 

Checks for calls to `ssl.wrap_socket` which have no `ssl_version` argument set. See also https://bandit.readthedocs.io/en/latest/_modules/bandit/plugins/insecure_ssl_tls.html#ssl_with_no_version

## Test Plan

Fixture added 

## Issue Link

Refers: https://github.com/astral-sh/ruff/issues/1646


---

_@charliermarsh reviewed on 2024-01-03 20:41_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_imports.rs`:16 on 2024-01-03 20:41_

Nice work by me!

---

_Label `rule` added by @charliermarsh on 2024-01-03 21:34_

---

_Label `preview` added by @charliermarsh on 2024-01-03 21:34_

---

_@charliermarsh approved on 2024-01-03 21:34_

Thanks!

---

_Comment by @qdegraaf on 2024-01-03 21:49_

Thanks for quick review! Unsure what's up with linux and windows tests, is this a known CI issue? 

---

_Comment by @charliermarsh on 2024-01-03 21:50_

@qdegraaf - I think we just had to bump the rule set size.

---

_Merged by @charliermarsh on 2024-01-03 21:56_

---

_Closed by @charliermarsh on 2024-01-03 21:56_

---

_Comment by @github-actions[bot] on 2024-01-03 22:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2024-01-03 23:42_

---
