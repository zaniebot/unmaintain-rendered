```yaml
number: 7661
title: "fix(rules): improve S507 detection"
type: pull_request
state: merged
author: mkniewallner
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/detect-more-S507-misusages
created_at: 2023-09-25T21:51:20Z
updated_at: 2023-09-28T21:42:23Z
url: https://github.com/astral-sh/ruff/pull/7661
synced_at: 2026-01-12T15:55:24Z
```

# fix(rules): improve S507 detection

---

_@mkniewallner_

## Summary

Follow-up on https://github.com/astral-sh/ruff/pull/7528 that improves detections of mis-usages of policy in `paramiko`.

First commit applies the same fix as in `bandit` (https://github.com/PyCQA/bandit/pull/1064), as `paramiko` supports passing both a class and a class instance for the policy in `set_missing_host_key_policy` (https://github.com/paramiko/paramiko/blob/8e389c77660c5cdae3069b478665427d23012853/paramiko/client.py#L171-L191).

Second commit improve the detection of `paramiko` import paths that trigger a violation, as `AutoAddPolicy`, `WarningPolicy` and `SSHClient` are not only exposed in `paramiko.client`, but also in `paramiko` (https://github.com/paramiko/paramiko/blob/66117732de6de03914308f9a21b05b50a781d13c/paramiko/__init__.py#L121-L164).

## Test Plan

Snapshot tests.

---

_Comment by @github-actions[bot] on 2023-09-25 22:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @zanieb on 2023-09-26 13:47_

This looks good to me! Is there a reason it's still in draft?

---

_Comment by @mkniewallner on 2023-09-26 13:56_

> This looks good to me! Is there a reason it's still in draft?

Yes sorry, I ideally wanted to wait for [the PR](https://github.com/PyCQA/bandit/pull/1064) in `bandit` to be merged first, but if you think we can merge the one in Ruff regardless, I'd be happy to mark it as ready to review.

---

_Marked ready for review by @mkniewallner on 2023-09-28 06:45_

---

_Comment by @mkniewallner on 2023-09-28 06:45_

[PR in bandit](https://github.com/PyCQA/bandit/pull/1064) has been merged, so this is now ready for review.

---

_@charliermarsh approved on 2023-09-28 21:20_

Thanks!

---

_Label `bug` added by @charliermarsh on 2023-09-28 21:25_

---

_Merged by @charliermarsh on 2023-09-28 21:36_

---

_Closed by @charliermarsh on 2023-09-28 21:36_

---

_Branch deleted on 2023-09-28 21:40_

---
