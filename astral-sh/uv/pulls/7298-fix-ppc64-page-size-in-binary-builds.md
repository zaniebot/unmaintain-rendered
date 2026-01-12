```yaml
number: 7298
title: Fix PPC64 page size in binary builds.
type: pull_request
state: merged
author: tom-miller1
labels:
  - bug
  - releases
assignees: []
merged: true
base: main
head: main
created_at: 2024-09-11T16:27:36Z
updated_at: 2024-09-11T18:13:56Z
url: https://github.com/astral-sh/uv/pull/7298
synced_at: 2026-01-12T16:07:46Z
```

# Fix PPC64 page size in binary builds.

---

_@tom-miller1_

## Summary

Add maturin build flag to set 64kb page size on PPC64 and PPC64LE architectures. Not aware of modern systems that use 4kb pages.

Resolves #6528

## Test Plan

ppc64le gnu dynamic-linked and musl static-linked binary builds were tested successfully on an IBM Power9 system running RHEL 8.8. I do not have access to other types of PPC64 systems for testing.


---

_@charliermarsh approved on 2024-09-11 18:13_

Thanks!

---

_Merged by @charliermarsh on 2024-09-11 18:13_

---

_Closed by @charliermarsh on 2024-09-11 18:13_

---

_Label `bug` added by @charliermarsh on 2024-09-11 18:13_

---

_Label `releases` added by @charliermarsh on 2024-09-11 18:13_

---
