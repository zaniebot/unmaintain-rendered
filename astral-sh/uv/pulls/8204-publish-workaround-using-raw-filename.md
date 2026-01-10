```yaml
number: 8204
title: "Publish: Workaround using raw filename"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/raw-filename
created_at: 2024-10-15T09:02:02Z
updated_at: 2024-10-15T12:22:54Z
url: https://github.com/astral-sh/uv/pull/8204
synced_at: 2026-01-10T12:54:04Z
```

# Publish: Workaround using raw filename

---

_Pull request opened by @konstin on 2024-10-15 09:02_

Since setuptools violates the wheel filenames spec (https://github.com/pypa/setuptools/issues/3777), we have to use the raw filename when publishing to get through https://github.com/pypi/warehouse/blob/50a58f3081e693a3772c0283050a275e350004bf/warehouse/forklift/legacy.py#L1133-L1155.

Tested with

```
cargo run publish aviary.gsm8k-0.7.6-py3-none-any.whl --publish-url https://test.pypi.org/legacy/
```

Fixes #8030

---

_Label `bug` added by @konstin on 2024-10-15 09:02_

---

_Label `preview` added by @konstin on 2024-10-15 09:02_

---

_@charliermarsh approved on 2024-10-15 12:03_

---

_Merged by @konstin on 2024-10-15 12:22_

---

_Closed by @konstin on 2024-10-15 12:22_

---

_Branch deleted on 2024-10-15 12:22_

---
