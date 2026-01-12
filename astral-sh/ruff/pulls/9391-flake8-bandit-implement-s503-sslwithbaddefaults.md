```yaml
number: 9391
title: " [`flake8-bandit`] Implement `S503` `SslWithBadDefaults` rule"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule/S403
created_at: 2024-01-04T17:17:24Z
updated_at: 2024-01-05T01:45:39Z
url: https://github.com/astral-sh/ruff/pull/9391
synced_at: 2026-01-12T15:55:28Z
```

#  [`flake8-bandit`] Implement `S503` `SslWithBadDefaults` rule

---

_@qdegraaf_

## Summary

Adds S503 rule for the [flake8-bandit](https://github.com/tylerwince/flake8-bandit) plugin port.

Checks for function defs argument defaults which have an insecure ssl_version value. See also https://bandit.readthedocs.io/en/latest/_modules/bandit/plugins/insecure_ssl_tls.html#ssl_with_bad_defaults

Some logic and the `const` can be shared with https://github.com/astral-sh/ruff/pull/9390. When one of the two is merged.

## Test Plan

Fixture added

## Issue Link

Refers: https://github.com/astral-sh/ruff/issues/1646



---

_Comment by @github-actions[bot] on 2024-01-04 17:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @charliermarsh on 2024-01-04 19:21_

---

_Label `preview` added by @charliermarsh on 2024-01-04 19:21_

---

_@charliermarsh approved on 2024-01-05 01:32_

Great, thanks!

---

_Merged by @charliermarsh on 2024-01-05 01:38_

---

_Closed by @charliermarsh on 2024-01-05 01:38_

---
