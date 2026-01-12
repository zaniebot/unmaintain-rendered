```yaml
number: 2406
title: "Fast lint CI job: Rustfmt, Prettier, Ruff"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/format-yaml-files
created_at: 2024-03-13T10:35:21Z
updated_at: 2024-03-20T00:16:48Z
url: https://github.com/astral-sh/uv/pull/2406
synced_at: 2026-01-12T16:05:01Z
```

# Fast lint CI job: Rustfmt, Prettier, Ruff

---

_@konstin_

Add a single job for for fast lint tools. Rustfmt for rust, ruff for python formatting and linting, prettier avoids inconsistent formatter changes between pycharm and vscode.

---

_Label `internal` added by @konstin on 2024-03-13 10:35_

---

_Review requested from @charliermarsh by @konstin on 2024-03-13 10:35_

---

_Review requested from @zanieb by @konstin on 2024-03-13 10:35_

---

_Comment by @konstin on 2024-03-13 13:24_

Should we do a single task that does all the fast linters: rustfmt, prettier (yml), ruff format and ruff check? The github UI becomews unwieldy with too many jobs:

![grafik](https://github.com/astral-sh/uv/assets/6826232/f19db854-dd50-4717-9ee2-56676f3e8bfe)


---

_Comment by @zanieb on 2024-03-13 14:18_

I'd be down to have all the formatting run in a single job, yeah

---

_Review request for @charliermarsh removed by @konstin on 2024-03-18 09:38_

---

_Renamed from "Use prettier to enforce yaml formatting" to "Fast lint CI job: Rustfmt, Prettier, Ruff" by @konstin on 2024-03-18 09:39_

---

_Merged by @charliermarsh on 2024-03-20 00:16_

---

_Closed by @charliermarsh on 2024-03-20 00:16_

---

_Branch deleted on 2024-03-20 00:16_

---
