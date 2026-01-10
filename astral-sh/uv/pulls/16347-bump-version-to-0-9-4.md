```yaml
number: 16347
title: Bump version to 0.9.4
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/094
created_at: 2025-10-17T19:48:01Z
updated_at: 2025-10-18T14:34:00Z
url: https://github.com/astral-sh/uv/pull/16347
synced_at: 2026-01-10T06:36:16Z
```

# Bump version to 0.9.4

---

_Pull request opened by @zanieb on 2025-10-17 19:48_

_No description provided._

---

_Marked ready for review by @zanieb on 2025-10-17 21:01_

---

_Merged by @zanieb on 2025-10-17 21:02_

---

_Closed by @zanieb on 2025-10-17 21:02_

---

_Branch deleted on 2025-10-17 21:02_

---

_Comment by @ulgens on 2025-10-18 06:46_

@zanieb Would it be possible to push a Github release too? `setup-uv` (7.1.0) is failing to use 0.9.4, saying

```bash
[DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
manifest-file not provided, reading from local file.
manifest-file does not contain version 0.9.4, arch x86_64, platform unknown-linux-gnu. Falling back to GitHub releases.
Downloading uv from "https://github.com/astral-sh/uv/releases/download/0.9.4/uv-x86_64-unknown-linux-gnu.tar.gz" ...
Error: Unexpected HTTP response: 404
```

---

_Comment by @zanieb on 2025-10-18 14:34_

The release isn't out anywhere yet — we've been dealing with some errors from GitHub

---
