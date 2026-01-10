```yaml
number: 847
title: "Allow files >4GB on 32-bit platforms"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/filesize-u64
created_at: 2024-01-09T16:17:43Z
updated_at: 2024-01-09T16:31:50Z
url: https://github.com/astral-sh/uv/pull/847
synced_at: 2026-01-10T15:44:44Z
```

# Allow files >4GB on 32-bit platforms

---

_Pull request opened by @konstin on 2024-01-09 16:17_

Changes `File::size` from a `usize` to a `u64`.

The motivations are that with tensorflow wheels being 475 MB (https://pypi.org/project/tensorflow/2.15.0.post1/#files), we're already only one order of magnitude away and to avoid target dependent failures.

---

_@charliermarsh approved on 2024-01-09 16:18_

---

_Merged by @konstin on 2024-01-09 16:31_

---

_Closed by @konstin on 2024-01-09 16:31_

---

_Branch deleted on 2024-01-09 16:31_

---
