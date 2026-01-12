```yaml
number: 9390
title: " [`flake8-bandit`] Implement `S502` `SslInsecureVersion` rule"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule/S402
created_at: 2024-01-04T11:52:39Z
updated_at: 2024-01-05T01:35:06Z
url: https://github.com/astral-sh/ruff/pull/9390
synced_at: 2026-01-12T15:55:28Z
```

#  [`flake8-bandit`] Implement `S502` `SslInsecureVersion` rule

---

_@qdegraaf_

## Summary

Adds S502 rule for the [flake8-bandit](https://github.com/tylerwince/flake8-bandit) plugin port.

Checks for calls to any function with keywords arguments `ssl_version` or `method` or for kwargs `method` in calls to `OpenSSL.SSL.Context` and `ssl_version` in calls to `ssl.wrap_socket` which have an insecure ssl_version valu. See also https://bandit.readthedocs.io/en/latest/_modules/bandit/plugins/insecure_ssl_tls.html#ssl_with_bad_version

## Test Plan

Fixture added

## Issue Link

Refers: https://github.com/astral-sh/ruff/issues/1646

---

_@qdegraaf reviewed on 2024-01-04 11:56_

---

_Review comment by @qdegraaf on `crates/ruff_linter/src/rules/flake8_bandit/rules/ssl_insecure_version.rs`:68 on 2024-01-04 11:56_

Upstream implementations checks for **all** calls at MEDIUM severity and the the specific functions at HIGH severity. Unsure if we want to replicate this, as it might be a bit heavy performance wise and Ruff/flake8-bandit has no way of separating the severity levels right now. Just copied upstream implementation for now

---

_Comment by @github-actions[bot] on 2024-01-04 12:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @charliermarsh on 2024-01-04 16:30_

---

_Label `preview` added by @charliermarsh on 2024-01-04 16:30_

---

_@charliermarsh approved on 2024-01-05 01:20_

Awesome, thanks.

---

_@charliermarsh reviewed on 2024-01-05 01:20_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/ssl_insecure_version.rs`:68 on 2024-01-05 01:20_

I decided to reduce the rule scope for the same reason.

---

_Merged by @charliermarsh on 2024-01-05 01:27_

---

_Closed by @charliermarsh on 2024-01-05 01:27_

---
