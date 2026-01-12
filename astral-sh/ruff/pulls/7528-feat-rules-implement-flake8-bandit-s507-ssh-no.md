```yaml
number: 7528
title: "feat(rules): implement `flake8-bandit` `S507` (`ssh_no_host_key_verification`)"
type: pull_request
state: merged
author: mkniewallner
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/add-flake8-bandit-S507
created_at: 2023-09-19T23:47:43Z
updated_at: 2023-09-20T01:03:45Z
url: https://github.com/astral-sh/ruff/pull/7528
synced_at: 2026-01-12T15:55:24Z
```

# feat(rules): implement `flake8-bandit` `S507` (`ssh_no_host_key_verification`)

---

_@mkniewallner_

Part of #1646.

## Summary

Implement `S507` ([`ssh_no_host_key_verification`](https://bandit.readthedocs.io/en/latest/plugins/b507_ssh_no_host_key_verification.html)) rule from `bandit`.

## Test Plan

Snapshot test from https://github.com/PyCQA/bandit/blob/1.7.5/examples/no_host_key_verification.py, with several additions to test for more cases (most notably passing the parameter as a named argument).

---

_Marked ready for review by @mkniewallner on 2023-09-20 00:02_

---

_Comment by @github-actions[bot] on 2023-09-20 00:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-09-20 00:29_

This looks great, thanks!

---

_Label `rule` added by @charliermarsh on 2023-09-20 00:31_

---

_Merged by @charliermarsh on 2023-09-20 00:31_

---

_Closed by @charliermarsh on 2023-09-20 00:31_

---

_Branch deleted on 2023-09-20 01:03_

---
