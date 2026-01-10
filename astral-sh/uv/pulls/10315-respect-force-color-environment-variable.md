```yaml
number: 10315
title: "Respect `FORCE_COLOR` environment variable"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/ter
created_at: 2025-01-06T01:00:49Z
updated_at: 2025-01-06T02:18:17Z
url: https://github.com/astral-sh/uv/pull/10315
synced_at: 2026-01-10T11:44:42Z
```

# Respect `FORCE_COLOR` environment variable

---

_Pull request opened by @charliermarsh on 2025-01-06 01:00_

## Summary

Closes https://github.com/astral-sh/uv/issues/10303.


---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:179 on 2025-01-06 01:01_

The presence of the default meant we always used `Auto` :(

---

_@charliermarsh reviewed on 2025-01-06 01:01_

---

_Label `bug` added by @charliermarsh on 2025-01-06 01:01_

---

_Label `cli` added by @charliermarsh on 2025-01-06 01:01_

---

_@samypr100 reviewed on 2025-01-06 01:03_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:179 on 2025-01-06 01:03_

Does it need to be removed from https://github.com/astral-sh/uv/blob/7182a34aa414f788d1a76a15690f2cd14e398e77/crates/uv-cli/src/lib.rs#L4489 as well?

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:176 on 2025-01-06 01:07_

```suggestion
    /// By default, uv will use automatically detect support for colors when writing to a terminal.
```

---

_@samypr100 reviewed on 2025-01-06 01:07_

---

_@charliermarsh reviewed on 2025-01-06 01:08_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:179 on 2025-01-06 01:08_

I think it doesn't matter in practice, but yeah it makes sense to keep them in-sync. Thanks!

---

_Merged by @charliermarsh on 2025-01-06 02:18_

---

_Closed by @charliermarsh on 2025-01-06 02:18_

---

_Branch deleted on 2025-01-06 02:18_

---
