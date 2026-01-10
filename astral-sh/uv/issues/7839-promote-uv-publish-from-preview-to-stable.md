```yaml
number: 7839
title: "Promote `uv publish` from preview to stable"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-10-01T13:26:30Z
updated_at: 2025-02-13T22:17:52Z
url: https://github.com/astral-sh/uv/issues/7839
synced_at: 2026-01-10T03:50:30Z
```

# Promote `uv publish` from preview to stable

---

_Issue opened by @konstin on 2024-10-01 13:26_

`uv publish` is getting adopted and we fix all user reports. We should promote it from preview to stable. 

## Blocking

Checklist for promoting it to stable:

- [x] Migrate uv from `pypa/gh-action-pypi-publish` to `uv publish`: https://github.com/astral-sh/uv/pull/8065
- [x] Create an example repo that use uv to publish a package to pypi and one alternative index: https://github.com/astral-sh/trusted-publishing-examples
- [x] https://github.com/astral-sh/uv/issues/7727
- [x] https://github.com/astral-sh/uv/issues/7809
- [x] https://github.com/astral-sh/uv/issues/7917
- [x] https://github.com/astral-sh/uv/issues/7958
- [x] https://github.com/astral-sh/uv/issues/7963 (Documentation)
- [x] https://github.com/astral-sh/uv/issues/8023
- [x] https://github.com/astral-sh/uv/issues/8030
- [x] https://github.com/astral-sh/uv/issues/8073
- [x] https://github.com/astral-sh/uv/issues/8418 (the first case, the others are fixed already)
- [x] https://github.com/astral-sh/uv/issues/8214
- [x] https://github.com/astral-sh/uv/issues/8423
- [x] https://github.com/astral-sh/uv/issues/8781
- [x] https://github.com/astral-sh/uv/pull/8803
- [x] https://github.com/astral-sh/uv/issues/8944
- [x] https://github.com/astral-sh/uv/issues/8864
- [x] https://github.com/astral-sh/uv/issues/9778

(wait one or two releases)

- [ ] Promotion PR


## Not blocking

These items are part of `uv publish`, but don't block stabilizing the feature itself.

- https://github.com/astral-sh/uv/issues/9845
- Setuptools is writing an invalid combination of Metadata-Version and used metadata fields in some cases (https://github.com/astral-sh/uv/issues/9513).

## Not included

- https://github.com/astral-sh/uv/issues/8535 (Internal test flake)
- https://github.com/astral-sh/uv/issues/8641 We want this is in some capacity; I expect it to become part of the build backend or part of `build build`, doing it as part of publish is too late. It's not blocking stabilization.
- https://github.com/astral-sh/uv/issues/8774 We want this is in some capacity; I expect it to become part of the build backend or part of `build build`, doing it as part of publish is too late. It's not blocking stabilization.


---

_Label `enhancement` added by @konstin on 2024-10-01 13:26_

---

_Added to milestone `v0.5.0` by @zanieb on 2024-10-01 19:53_

---

_Label `preview` added by @konstin on 2024-10-28 10:26_

---

_Assigned to @konstin by @konstin on 2024-11-10 11:09_

---

_Removed from milestone `v0.5.0` by @konstin on 2024-11-10 11:09_

---

_Closed by @zanieb on 2025-02-13 22:17_

---
