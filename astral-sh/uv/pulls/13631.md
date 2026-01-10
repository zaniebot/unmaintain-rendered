```yaml
number: 13631
title: "Omit `license-file` key for Metadata 2.1"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/license-file
created_at: 2025-05-23T23:59:30Z
updated_at: 2025-09-22T12:55:17Z
url: https://github.com/astral-sh/uv/pull/13631
synced_at: 2026-01-10T06:36:15Z
```

# Omit `license-file` key for Metadata 2.1

---

_Pull request opened by @charliermarsh on 2025-05-23 23:59_

## Summary

Some versions of `setuptools` added this key despite using Metadata 2.1. It seems like `setuptools` now writes Metadata 2.4? But this is a challenging error if you have existing wheels that you want to upload.

See: https://github.com/pypa/setuptools/issues/4759

Closes #9513

---

_Review requested from @konstin by @charliermarsh on 2025-05-23 23:59_

---

_Comment by @charliermarsh on 2025-05-28 00:27_

Chatted with @konstin and he convinced me not to support this :)

---

_Closed by @charliermarsh on 2025-05-28 00:27_

---

_Comment by @alexanderankin on 2025-09-22 12:53_

just realized you guys chose not to support this, but twine just failed validation for me, so im confused as to how this is considered valid behavior still.

---

_Comment by @konstin on 2025-09-22 12:55_

This is a bug in setuptools, and needs to be fixed on the setuptools side, to avoid creating packages with invalid metadata: https://github.com/pypa/setuptools/issues/4759

---
