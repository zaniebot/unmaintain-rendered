```yaml
number: 7703
title: "feat(rules): implement `flake8-bandit` `S505`"
type: pull_request
state: merged
author: mkniewallner
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/add-flake8-bandit-S505
created_at: 2023-09-28T21:13:12Z
updated_at: 2023-09-29T06:08:19Z
url: https://github.com/astral-sh/ruff/pull/7703
synced_at: 2026-01-12T15:55:24Z
```

# feat(rules): implement `flake8-bandit` `S505`

---

_@mkniewallner_

Part of #1646.

## Summary

Implement `S505` ([`weak_cryptographic_key`](https://bandit.readthedocs.io/en/latest/plugins/b505_weak_cryptographic_key.html)) rule from `bandit`.

For this rule, `bandit` [reports the issue with](https://github.com/PyCQA/bandit/blob/1.7.5/bandit/plugins/weak_cryptographic_key.py#L47-L56):
- medium severity for DSA/RSA < 2048 bits and EC < 224 bits
- high severity for DSA/RSA < 1024 bits and EC < 160 bits

Since Ruff does not handle severities for `bandit`-related rules, we could either report the issue if we have lower values than medium severity, or lower values than high one. Two reasons led me to choose the first option:
- a medium severity issue is still a security issue we would want to report to the user, who can then decide to either handle the issue or ignore it
- `bandit` [maps the EC key algorithms to their respective key lengths in bits](https://github.com/PyCQA/bandit/blob/1.7.5/bandit/plugins/weak_cryptographic_key.py#L112-L133), but there is no value below 160 bits, so technically `bandit` would never report medium severity issues for EC keys, only high ones

Another consideration is that as shared just above, for EC key algorithms, `bandit` has a mapping to map the algorithms to their respective key lengths. In the implementation in Ruff, I rather went with an explicit list of EC algorithms known to be vulnerable (which would thus be reported) rather than implementing a mapping to retrieve the associated key length and comparing it with the minimum value.

## Test Plan

Snapshot tests from https://github.com/PyCQA/bandit/blob/1.7.5/examples/weak_cryptographic_key_sizes.py.

---

_Comment by @github-actions[bot] on 2023-09-28 21:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Marked ready for review by @mkniewallner on 2023-09-28 21:36_

---

_Comment by @codspeed-hq[bot] on 2023-09-28 23:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mkniewallner:feat/add-flake8-bandit-S505)

### Merging #7703 will **not alter performance**

<sub>Comparing <code>mkniewallner:feat/add-flake8-bandit-S505</code> (e2e94f7) with <code>main</code> (c2a9cf8)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_@charliermarsh approved on 2023-09-28 23:21_

Thanks!

---

_Comment by @charliermarsh on 2023-09-28 23:23_

> Two reasons led me to choose the first option...

FWIW I agree with this.

---

_Merged by @charliermarsh on 2023-09-29 01:27_

---

_Closed by @charliermarsh on 2023-09-29 01:27_

---

_Label `rule` added by @charliermarsh on 2023-09-29 01:27_

---

_Branch deleted on 2023-09-29 06:08_

---
