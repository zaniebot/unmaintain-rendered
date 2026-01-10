```yaml
number: 9110
title: Add support for async unzipping with ZIP64 archives
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/eocdr
created_at: 2024-11-14T02:22:16Z
updated_at: 2024-11-14T21:45:42Z
url: https://github.com/astral-sh/uv/pull/9110
synced_at: 2026-01-10T12:00:00Z
```

# Add support for async unzipping with ZIP64 archives

---

_Pull request opened by @charliermarsh on 2024-11-14 02:22_

## Summary

See: https://github.com/charliermarsh/rs-async-zip/pull/4.

Closes https://github.com/astral-sh/uv/issues/8031.

## Test Plan

I created a wheel with 100,000 files in it.

I verified that `uv pip install https://github.com/astral-sh/uv/raw/refs/heads/charlie/sixtyfour/sixtyfour/dist/sixtyfour-0.1.0-py3-none-any.whl` fails, while `cargo run pip install https://github.com/astral-sh/uv/raw/refs/heads/charlie/sixtyfour/sixtyfour/dist/sixtyfour-0.1.0-py3-none-any.whl` succeeds, and I can then `import sixtyfour`, `import sixtyfour.file_20557`, etc.


---

_Label `bug` added by @charliermarsh on 2024-11-14 02:22_

---

_Marked ready for review by @charliermarsh on 2024-11-14 02:22_

---

_@zanieb approved on 2024-11-14 04:54_

Can you change to a revision?

---

_Comment by @charliermarsh on 2024-11-14 05:17_

Oh yeah sorry, I was hoping to get this tested by the issue author before merging either PR.

---

_Comment by @charliermarsh on 2024-11-14 21:34_

Ok, I created a wheel with 100,000 files in it. I verified that `uv pip install https://github.com/astral-sh/uv/raw/refs/heads/charlie/sixtyfour/sixtyfour/dist/sixtyfour-0.1.0-py3-none-any.whl` fails, while `cargo run pip install https://github.com/astral-sh/uv/raw/refs/heads/charlie/sixtyfour/sixtyfour/dist/sixtyfour-0.1.0-py3-none-any.whl` succeeds, and I can then `import sixtyfour`, `import sixtyfour.file_20557`, etc.

---

_Merged by @charliermarsh on 2024-11-14 21:45_

---

_Closed by @charliermarsh on 2024-11-14 21:45_

---

_Branch deleted on 2024-11-14 21:45_

---
