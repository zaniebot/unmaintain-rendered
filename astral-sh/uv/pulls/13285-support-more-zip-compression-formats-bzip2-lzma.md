```yaml
number: 13285
title: "Support more zip compression formats: bzip2, lzma, xz, zstd"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: konsti/more-zip-compressions
created_at: 2025-05-04T16:02:05Z
updated_at: 2025-05-04T18:08:21Z
url: https://github.com/astral-sh/uv/pull/13285
synced_at: 2026-01-10T11:10:41Z
```

# Support more zip compression formats: bzip2, lzma, xz, zstd

---

_Pull request opened by @konstin on 2025-05-04 16:02_

Wheels are zip files, and as such can internally be compressed with a number of compression algorithms besides the popular choices, DEFLATE and stored. I added all algorithms supported by async-zip except `deflate64`, which wasn't yet a part of our dependency tree.  All other compression algorithms and crates are already supported and dependencies for their source dist `.tar.<format>` support.

Python 3.13 supports stored, deflate, bzip2 and lzma (https://docs.python.org/3/library/zipfile.html#zipfile.ZIP_STORED), PEP 784 adds zstandard support in 3.14.

Fixes #13192

---

_Label `enhancement` added by @konstin on 2025-05-04 16:02_

---

_Label `compatibility` added by @konstin on 2025-05-04 16:02_

---

_Review requested from @charliermarsh by @konstin on 2025-05-04 16:02_

---

_@charliermarsh approved on 2025-05-04 16:04_

---

_Merged by @konstin on 2025-05-04 18:08_

---

_Closed by @konstin on 2025-05-04 18:08_

---

_Branch deleted on 2025-05-04 18:08_

---
