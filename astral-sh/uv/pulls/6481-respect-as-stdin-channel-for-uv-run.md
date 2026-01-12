```yaml
number: 6481
title: "Respect `-` as stdin channel for `uv run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/stdin
created_at: 2024-08-23T00:50:49Z
updated_at: 2024-08-23T15:49:58Z
url: https://github.com/astral-sh/uv/pull/6481
synced_at: 2026-01-12T16:07:23Z
```

# Respect `-` as stdin channel for `uv run`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/6467.


---

_Label `cli` added by @charliermarsh on 2024-08-23 00:50_

---

_Marked ready for review by @charliermarsh on 2024-08-23 00:50_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-23 01:23_

---

_@zanieb reviewed on 2024-08-23 12:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:847 on 2024-08-23 12:59_

Seems kind of problematic to read the entire stream into a buffer? Won't this be problematic if the script is a big file? Shouldn't we be piping it to the child process?

---

_@zanieb approved on 2024-08-23 13:00_

A bit concerned about the way we handle the stream.

---

_Comment by @billy-doyle on 2024-08-23 14:12_

added to guide doc as well https://github.com/astral-sh/uv/pull/6519

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:847 on 2024-08-23 15:49_

I will take this in a follow-up if that's ok.

---

_@charliermarsh reviewed on 2024-08-23 15:49_

---

_@charliermarsh reviewed on 2024-08-23 15:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:847 on 2024-08-23 15:49_

(We do this for `requirements.txt` which is where I copied from, but you're right that it could be improved.)

---

_Merged by @charliermarsh on 2024-08-23 15:49_

---

_Closed by @charliermarsh on 2024-08-23 15:49_

---

_Branch deleted on 2024-08-23 15:49_

---
