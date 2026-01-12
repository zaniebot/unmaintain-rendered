```yaml
number: 1771
title: "[`flake8-bandit`] Add Rule for `S508` (snmp insecure version) & `S509` (snmp weak cryptography)"
type: pull_request
state: merged
author: saadmk11
labels: []
assignees: []
merged: true
base: main
head: add-s508-s509
created_at: 2023-01-10T14:15:44Z
updated_at: 2023-01-10T19:06:50Z
url: https://github.com/astral-sh/ruff/pull/1771
synced_at: 2026-01-12T15:55:07Z
```

# [`flake8-bandit`] Add Rule for `S508` (snmp insecure version) & `S509` (snmp weak cryptography)

---

_@saadmk11_

ref: https://github.com/charliermarsh/ruff/issues/1646

---

_Review comment by @messense on `src/violations.rs`:4809 on 2023-01-10 15:09_

```suggestion
    pub struct SnmpWeakCryptography;
```

---

_Review comment by @messense on `src/violations.rs`:4811 on 2023-01-10 15:09_

```suggestion
impl Violation for SnmpWeakCryptography {
```

---

_Review comment by @messense on `src/violations.rs`:4818 on 2023-01-10 15:09_

```suggestion
        SnmpWeakCryptography
```

---

_Review comment by @messense on `src/registry.rs`:420 on 2023-01-10 15:10_

```suggestion
    S509 => violations::SnmpWeakCryptography,
```

---

_Review comment by @messense on `src/flake8_bandit/rules/snmp_weak_cryptography.rs`:24 on 2023-01-10 15:10_

```suggestion
                violations::SnmpWeakCryptography,
```

---

_@messense reviewed on 2023-01-10 15:11_

Typo: `SnmpWeakCriptography` should be `SnmpWeakCryptography`

---

_Comment by @saadmk11 on 2023-01-10 15:33_

Making a lot of typo's today 😅

---

_Merged by @charliermarsh on 2023-01-10 18:13_

---

_Closed by @charliermarsh on 2023-01-10 18:13_

---

_Branch deleted on 2023-01-10 19:06_

---
