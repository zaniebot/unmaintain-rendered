```yaml
number: 2009
title: Use Metadata 2.2+ for source distributions
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - good first issue
assignees: []
created_at: 2024-02-27T10:21:09Z
updated_at: 2024-03-08T16:02:33Z
url: https://github.com/astral-sh/uv/issues/2009
synced_at: 2026-01-10T05:40:32Z
```

# Use Metadata 2.2+ for source distributions

---

_Issue opened by @konstin on 2024-02-27 10:21_

When a source distribution contains a PKG-INFO file at the top level with `Metadata-Version: 2.2` or `Metadata-Version: 2.3` and neither `Dynamic: Requires-Dist` nor `Dynamic: Provides-Extra`, we can use the information in PKG-INFO as reliable metadata without calling any PEP 517 hooks or even setting up the build env. While the PEP has been accepted for some time, this is now supported by at pypi (https://github.com/pypi/warehouse/pull/13606).

After downloading and unpacking[^1], we can read `PKG-INFO` with the same utils as `METADATA` and given the preconditions perform no build in `uv pip compile`. This should provide a significant speedup as more package are published with metadata 2.2+. 

[pyo3-mixed 2.1.5](https://inspector.pypi.io/project/pyo3-mixed/2.1.5/packages/2b/b8/e04b783d3569d5b61b1dcdfda683ac2e3617340539aecd0f099fbade0b4a/pyo3_mixed-2.1.5.tar.gz/pyo3_mixed-2.1.5/PKG-INFO)

[^1]: I don't think .tar.gz has a central directory we could use for the same remote partial reading trick as for zips, but i can't find a good source.

---

_Label `enhancement` added by @konstin on 2024-02-27 10:21_

---

_Comment by @charliermarsh on 2024-02-27 15:05_

Yes!!!

---

_Comment by @sbidoul on 2024-02-27 16:11_

~Note `Version` can be dynamic too.~

Update: no sorry, version can be dynamic in `pyproject.toml` (PEP 621). but not in PKG-INFO 2.2+. PEP 643 says "The fields Name and Version MUST NOT be marked as Dynamic."



---

_Label `good first issue` added by @charliermarsh on 2024-03-07 23:01_

---

_Comment by @charliermarsh on 2024-03-08 01:52_

I might do this because it looks fun.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-08 02:22_

---

_Closed by @charliermarsh on 2024-03-08 16:02_

---
