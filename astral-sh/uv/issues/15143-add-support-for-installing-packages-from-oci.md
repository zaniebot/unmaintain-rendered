---
number: 15143
title: Add support for installing packages from OCI registries
type: issue
state: open
author: lijok
labels:
  - wish
assignees: []
created_at: 2025-08-07T18:27:50Z
updated_at: 2025-08-26T15:34:36Z
url: https://github.com/astral-sh/uv/issues/15143
synced_at: 2026-01-10T01:25:53Z
---

# Add support for installing packages from OCI registries

---

_Issue opened by @lijok on 2025-08-07 18:27_

### Summary

Haven't seen this in the python space but generally we're moving towards OCI for all artifacts using something like [oras](https://github.com/oras-project/oras) currently to upload/download artifacts
Would be sweet if uv natively supported pulling from OCI registries

### Example

_No response_

---

_Label `enhancement` added by @lijok on 2025-08-07 18:27_

---

_Comment by @cpressland on 2025-08-20 11:54_

Agreed, I currently maintain a simple FastAPI service using ORAS to translate the Python Packaging API to OCI for work. It works well enough but having native support would be a huge benefit and allow Python Packages to be stored on GitHub Packages, etc.

---

_Comment by @jorgehermo9 on 2025-08-21 18:31_

+1, it would be nice to have an alternative to python package indexes. `uv publish` to upload to an oci registry and also allow `[tool.uv.index]` to specify images of oci registries would be awesome. 

but this would be a **major** initiative

Very similar idea: https://github.com/AllexVeldman/pyoci



---

_Label `wish` added by @konstin on 2025-08-26 15:34_

---

_Label `enhancement` removed by @konstin on 2025-08-26 15:34_

---
