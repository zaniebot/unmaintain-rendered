```yaml
number: 12724
title: Include docs requirements for Renovate upgrades
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ci
assignees: []
merged: true
base: main
head: dhruv/renovate-docs-req
created_at: 2024-08-07T04:24:54Z
updated_at: 2024-08-07T07:41:20Z
url: https://github.com/astral-sh/ruff/pull/12724
synced_at: 2026-01-10T21:47:02Z
```

# Include docs requirements for Renovate upgrades

---

_Pull request opened by @dhruvmanila on 2024-08-07 04:24_

## Summary

This PR updates the Renovate config to account for the `requirements*.txt` files in `docs/` directory.

The `mkdocs-material` upgrade is ignored because we use commit SHA for the insider version and it should match the corresponding public version as per the docs: https://squidfunk.github.io/mkdocs-material/insiders/upgrade/ (`9.x.x-insiders-4.x.x`).

## Test Plan

```console
❯ renovate-config-validator
(node:83193) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully
```


---

_Label `ci` added by @dhruvmanila on 2024-08-07 04:24_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-07 04:24_

---

_@AlexWaygood approved on 2024-08-07 06:59_

LGTM.

---

_Merged by @dhruvmanila on 2024-08-07 07:41_

---

_Closed by @dhruvmanila on 2024-08-07 07:41_

---

_Branch deleted on 2024-08-07 07:41_

---
