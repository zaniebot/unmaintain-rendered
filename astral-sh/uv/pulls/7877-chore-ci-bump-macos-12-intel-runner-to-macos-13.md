```yaml
number: 7877
title: "chore(ci): bump macos-12 intel runner to macos-13 intel runner"
type: pull_request
state: merged
author: samypr100
labels:
  - releases
assignees: []
merged: true
base: main
head: avoid-using-macos-12-in-ci
created_at: 2024-10-02T18:40:52Z
updated_at: 2024-10-11T02:37:23Z
url: https://github.com/astral-sh/uv/pull/7877
synced_at: 2026-01-12T16:08:03Z
```

# chore(ci): bump macos-12 intel runner to macos-13 intel runner

---

_@samypr100_

## Summary

Closes https://github.com/astral-sh/uv/issues/6972

This is not breaking since `MACOSX_DEPLOYMENT_TARGET` will stay the same (currently defaulting to 10.12) so a `uv-x.y.z-py3-none-macosx_10_12_x86_64.whl` will still be built


---

_@zanieb approved on 2024-10-02 18:42_

---

_Label `releases` added by @zanieb on 2024-10-02 18:42_

---

_Marked ready for review by @samypr100 on 2024-10-02 18:42_

---

_Renamed from "chore(ci): bump macos-12 intel runner to macos-13" to "chore(ci): bump macos-12 intel runner to macos-13 intel runner" by @samypr100 on 2024-10-02 18:43_

---

_Comment by @charliermarsh on 2024-10-03 12:00_

I've had bad experiences bumping these in the past (https://github.com/astral-sh/ruff/pull/9834) but hopefully not an issue. (I know this is different since it's Intel-to-Intel.)

---

_Merged by @charliermarsh on 2024-10-03 12:00_

---

_Closed by @charliermarsh on 2024-10-03 12:00_

---

_Branch deleted on 2024-10-11 02:37_

---
