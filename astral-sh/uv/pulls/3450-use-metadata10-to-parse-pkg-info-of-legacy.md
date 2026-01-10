```yaml
number: 3450
title: Use Metadata10 to parse PKG-INFO of legacy editable
type: pull_request
state: merged
author: hauntsaninja
labels:
  - bug
assignees: []
merged: true
base: main
head: uv-metadata10
created_at: 2024-05-08T05:52:11Z
updated_at: 2024-05-08T14:34:15Z
url: https://github.com/astral-sh/uv/pull/3450
synced_at: 2026-01-10T14:37:54Z
```

# Use Metadata10 to parse PKG-INFO of legacy editable

---

_Pull request opened by @hauntsaninja on 2024-05-08 05:52_

It turns out setuptools often uses Metadata-Version 2.1 in their PKG-INFO: https://github.com/hauntsaninja/setuptools/blob/4e766834d72623f3b938f1d4148547ea73af1bf5/setuptools/dist.py#L64
`Metadata23` requires Metadata-Version of at least 2.2.

This means that uv doesn't actually recognise legacy editable installations from the most common way you'd actually get legacy editable installations (works great for most legacy editables I make at work though!)

Anyway, over here we only need the version and don't care about anything else. Rather than make a `Metadata21`, I just add a version field to `Metadata10`. The one slightly tricky thing is that only Metadata-Version 1.2 and greater guarantee that the [version number is PEP 440 compatible](https://peps.python.org/pep-0345/#version), so I store the version in `Metadata10` as a `String` and only parse to `Version` at time of use.

Also did you know that back in 2004, paramiko had a pokemon based versioning system?

---

_@konstin approved on 2024-05-08 08:58_

> Also did you know that back in 2004, paramiko had a pokemon based versioning system?

Bring it back

---

_Merged by @konstin on 2024-05-08 08:58_

---

_Closed by @konstin on 2024-05-08 08:58_

---

_@konstin reviewed on 2024-05-08 09:07_

---

_Review comment by @konstin on `crates/uv/tests/pip_list.rs`:672 on 2024-05-08 09:07_

Error message fixed in https://github.com/astral-sh/uv/pull/3453

---

_Branch deleted on 2024-05-08 09:40_

---

_Label `bug` added by @charliermarsh on 2024-05-08 14:34_

---

_Comment by @charliermarsh on 2024-05-08 14:34_

Thanks! LGTM.

---
