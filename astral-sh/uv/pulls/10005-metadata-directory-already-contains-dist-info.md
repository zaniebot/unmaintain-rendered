```yaml
number: 10005
title: "`metadata_directory` already contains dist-info directory"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/fix-metadata-step-paths
created_at: 2024-12-18T15:45:41Z
updated_at: 2024-12-18T19:14:11Z
url: https://github.com/astral-sh/uv/pull/10005
synced_at: 2026-01-12T16:09:04Z
```

# `metadata_directory` already contains dist-info directory

---

_@konstin_

From PEP 517:

```python
def prepare_metadata_for_build_wheel(metadata_directory, config_settings=None):
    ...
```

> Must create a .dist-info directory containing wheel metadata inside the specified metadata_directory (i.e., creates a directory like {metadata_directory}/{package}-{version}.dist-info/).

```python
def build_wheel(wheel_directory, config_settings=None, metadata_directory=None):
    ...
```

> If the build frontend has previously called prepare_metadata_for_build_wheel and depends on the wheel resulting from this call to have metadata matching this earlier call, then it should provide the path to the created .dist-info directory as the metadata_directory argument.

Notice that the `metadata_directory` is different for the both hooks: For `prepare_metadata_for_build_wheel` is doesn't contain the `.dist-info` directory as final segment, for `build_wheel` it does.

Previously, the code assumed that both directories didn't contain the `.dist-info` for both cases.

Checked with:

```
maturin build
uv init test-uv-build-backend --build-backend uv
cd test-uv-build-backend
uv build --sdist --preview
cd ..
UV_PREVIEW=1 pip install test-uv-build-backend/dist/test_uv_build_backend-0.1.0.tar.gz --no-index --find-links target/wheels/ -v --no-cache-dir
```

Fixes #9969

---

_Review requested from @BurntSushi by @konstin on 2024-12-18 15:45_

---

_@zanieb approved on 2024-12-18 16:00_

---

_@BurntSushi approved on 2024-12-18 16:43_

---

_Label `bug` added by @konstin on 2024-12-18 19:14_

---

_Label `preview` added by @konstin on 2024-12-18 19:14_

---

_Merged by @konstin on 2024-12-18 19:14_

---

_Closed by @konstin on 2024-12-18 19:14_

---

_Branch deleted on 2024-12-18 19:14_

---
