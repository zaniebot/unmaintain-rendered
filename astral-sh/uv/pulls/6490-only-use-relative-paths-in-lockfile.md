```yaml
number: 6490
title: Only use relative paths in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - internal
assignees: []
merged: true
base: main
head: charlie/relative-path
created_at: 2024-08-23T04:12:30Z
updated_at: 2024-08-24T02:19:12Z
url: https://github.com/astral-sh/uv/pull/6490
synced_at: 2026-01-10T13:09:51Z
```

# Only use relative paths in lockfile

---

_Pull request opened by @charliermarsh on 2024-08-23 04:12_

For users who were using absolute paths in the `pyproject.toml` previously, this is a behavior change: We now convert all absolute paths in `path` entries to relative paths. Since i assume that no-one relies on absolute path in their lockfiles - they are intended to be portable - I'm tagging this as a bugfix.

Closes https://github.com/astral-sh/uv/pull/6438
Fixes https://github.com/astral-sh/uv/issues/6371

---

_Label `internal` added by @charliermarsh on 2024-08-23 04:12_

---

_Label `lock` added by @charliermarsh on 2024-08-23 04:12_

---

_Label `lock` removed by @konstin on 2024-08-23 20:45_

---

_Label `bug` added by @konstin on 2024-08-23 20:45_

---

_Marked ready for review by @konstin on 2024-08-23 20:52_

---

_Renamed from "Convert to relative paths at lock-time" to "Only use relative paths in lockfile" by @konstin on 2024-08-23 20:53_

---

_Comment by @charliermarsh on 2024-08-23 21:36_

I think we have a problem whereby distribution / requirement `install_path` is canonicalized (so symlinks are resolved), but the workspace install path is not...

---

_Comment by @charliermarsh on 2024-08-23 21:49_

I think we're too inconsistent about this unfortunately. Hmm...

---

_Comment by @charliermarsh on 2024-08-23 21:52_

I suspect all the Windows tests will still fail?

---

_Comment by @charliermarsh on 2024-08-23 22:54_

Trying to make this work but will later need to be broken up into multiple PRs.

---

_Merged by @charliermarsh on 2024-08-24 02:19_

---

_Closed by @charliermarsh on 2024-08-24 02:19_

---

_Branch deleted on 2024-08-24 02:19_

---
