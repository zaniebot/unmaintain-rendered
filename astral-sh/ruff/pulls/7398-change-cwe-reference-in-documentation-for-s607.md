```yaml
number: 7398
title: Change CWE reference in documentation for S607 rule
type: pull_request
state: merged
author: manueljacob
labels:
  - documentation
assignees: []
merged: true
base: main
head: S607-CWE_ref
created_at: 2023-09-15T03:51:26Z
updated_at: 2023-09-15T04:12:54Z
url: https://github.com/astral-sh/ruff/pull/7398
synced_at: 2026-01-12T02:39:10Z
```

# Change CWE reference in documentation for S607 rule

---

_Pull request opened by @manueljacob on 2023-09-15 03:51_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The previous reference was “CWE-78: Improper Neutralization of Special Elements used in an OS Command ('OS Command Injection')”, which describes another issue. The new reference is “CWE-426: Untrusted Search Path”, which describes exactly the problem that this rule should warn about.

## Test Plan

The change was not tested, as it only changes two numbers in the documentation.

---

_Comment by @github-actions[bot] on 2023-09-15 04:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-09-15 04:12_

Thanks!

---

_Label `documentation` added by @zanieb on 2023-09-15 04:12_

---

_Merged by @zanieb on 2023-09-15 04:12_

---

_Closed by @zanieb on 2023-09-15 04:12_

---
