```yaml
number: 17520
title: "Gate `build_backend::build_with_all_metadata` on `pypi` feature"
type: pull_request
state: merged
author: musicinmybrain
labels:
  - testing
assignees: []
merged: true
base: main
head: pypi-0.9.26
created_at: 2026-01-16T14:16:18Z
updated_at: 2026-01-16T14:25:48Z
url: https://github.com/astral-sh/uv/pull/17520
synced_at: 2026-01-16T14:58:08Z
```

# Gate `build_backend::build_with_all_metadata` on `pypi` feature

---

_@musicinmybrain_

## Summary

This test, added in 0.9.26, tries to download anyio from PyPI. This PR therefore gates it on the `pypi` feature.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Applied as a patch to the `uv` package in Fedora and built in an offline environment.

---

_@zanieb approved on 2026-01-16 14:16_

---

_Label `testing` added by @zanieb on 2026-01-16 14:17_

---

_@konstin approved on 2026-01-16 14:17_

Sorry I missed that

---

_Renamed from "Gate build_backend::build_with_all_metadata on pypi feature" to "Gate `build_backend::build_with_all_metadata` on `pypi` feature" by @zanieb on 2026-01-16 14:17_

---

_Merged by @zanieb on 2026-01-16 14:25_

---

_Closed by @zanieb on 2026-01-16 14:25_

---
