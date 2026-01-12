```yaml
number: 10618
title: "feat: add `--no-default-groups` flag"
type: pull_request
state: merged
author: mkniewallner
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: feat/add-no-groups-flag
created_at: 2025-01-14T23:29:24Z
updated_at: 2025-01-21T23:07:19Z
url: https://github.com/astral-sh/uv/pull/10618
synced_at: 2026-01-12T16:09:23Z
```

# feat: add `--no-default-groups` flag

---

_@mkniewallner_

## Summary

Closes #10592.

## Test Plan

Snapshot tests.

---

_Comment by @zanieb on 2025-01-14 23:41_

Should we have `--no-groups` or `--no-default-groups`?

---

_Comment by @zanieb on 2025-01-14 23:41_

I think the latter might be more versatile.

---

_Comment by @mkniewallner on 2025-01-14 23:45_

Would `--no-default-groups` disable `tool.uv.default-groups` under the hood, meaning that it would set it back to `dev` section only by default? If so, I believe the implementation should be changed a bit, as in that case we should not exclude all groups, but include `dev` group unless `--no-dev` is also specified?

---

_Comment by @zanieb on 2025-01-14 23:47_

I think it would exclude all groups provided via `default-groups` including our default dev behavior. The difference would be like...

- `--group foo --group bar --no-groups`: no groups selected
- `--group foo --group bar --no-default-groups`: foo and bar still selected

---

_Renamed from "feat: add `--no-groups` flag" to "feat: add `--no-default-groups` flag" by @mkniewallner on 2025-01-15 01:04_

---

_Marked ready for review by @mkniewallner on 2025-01-15 01:09_

---

_@mkniewallner reviewed on 2025-01-15 01:11_

---

_Review comment by @mkniewallner on `crates/uv-configuration/src/dev.rs`:71 on 2025-01-15 01:11_

Didn't find a better naming and description.

---

_Review requested from @charliermarsh by @zanieb on 2025-01-15 19:04_

---

_Assigned to @charliermarsh by @zanieb on 2025-01-15 19:04_

---

_Comment by @cjw296 on 2025-01-21 09:19_

@zanieb - as you can see from the issue @charliermarsh linked, I've run into having a selection of default groups but only wanted to install _only_ one but _including_ the project, I think this PR should make it possible with:

```
uv sync --no-default-groups --group docs
```

This still feels a bit clunky, so open to other suggestions, but will use it if there's nothing else on offer :-) 

---

_@charliermarsh approved on 2025-01-21 22:51_

Thanks!

---

_Merged by @charliermarsh on 2025-01-21 23:03_

---

_Closed by @charliermarsh on 2025-01-21 23:03_

---

_Label `enhancement` added by @charliermarsh on 2025-01-21 23:03_

---

_Label `cli` added by @charliermarsh on 2025-01-21 23:03_

---

_Branch deleted on 2025-01-21 23:07_

---
