```yaml
number: 10602
title: Update warning message for rule S305 to address insecure block cipher mode use
type: pull_request
state: merged
author: mnixry
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-03-26T08:29:23Z
updated_at: 2024-03-28T01:00:49Z
url: https://github.com/astral-sh/ruff/pull/10602
synced_at: 2026-01-10T22:47:02Z
```

# Update warning message for rule S305 to address insecure block cipher mode use

---

_Pull request opened by @mnixry on 2024-03-26 08:29_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR updates the warning message for rule S305 to accurately reflect the security concern over using ECB mode in block ciphers, which is considered insecure compared to other modes like CBC or CTR. The previous message incorrectly mentioned AES as a [block cipher mode](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation), which has been corrected to avoid confusion.

Ref: 
https://github.com/PyCQA/bandit/blob/c85576d903c213859634f4fbcb6304042442e62a/bandit/blacklists/calls.py#L99-L102

https://github.com/astral-sh/ruff/blob/825fd7c990e12569886127f7f7ca953b5516d327/crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs#L187-L216

## Test Plan

No testing required as the change is limited to a minor change of warning message update.


---

_Comment by @github-actions[bot] on 2024-03-26 08:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-03-28 01:00_

Thanks, this looks right to me!

---

_Label `documentation` added by @charliermarsh on 2024-03-28 01:00_

---

_Merged by @charliermarsh on 2024-03-28 01:00_

---

_Closed by @charliermarsh on 2024-03-28 01:00_

---
