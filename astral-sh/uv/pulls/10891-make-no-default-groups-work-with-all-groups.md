```yaml
number: 10891
title: "Make `--no-default-groups` work with `--all-groups`"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
base: main
head: groups-cli
created_at: 2025-01-23T09:56:11Z
updated_at: 2025-02-05T20:33:02Z
url: https://github.com/astral-sh/uv/pull/10891
synced_at: 2026-01-10T11:10:34Z
```

# Make `--no-default-groups` work with `--all-groups`

---

_Pull request opened by @j178 on 2025-01-23 09:56_

## Summary

Closes #10890



---

_@zanieb reviewed on 2025-01-23 14:12_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2714 on 2025-01-23 14:12_

I don't think these need to conflict, they're just redundant? (`--group foo --all-groups`)

---

_@zanieb reviewed on 2025-01-23 14:13_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2726 on 2025-01-23 14:13_

👍 yeah this was wrong

Similarly, I think `--only-group` is just redundant? `--only-group foo --no-default-groups` doesn't need to error.

---

_@zanieb reviewed on 2025-01-23 14:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2734 on 2025-01-23 14:14_

Agree this should conflict with `--all-groups`

---

_Renamed from "Make `--no-default-groups` works with `--all-groups`" to "Make `--no-default-groups` work with `--all-groups`" by @j178 on 2025-01-24 07:29_

---

_@j178 reviewed on 2025-01-24 08:46_

---

_Review comment by @j178 on `crates/uv-cli/src/lib.rs`:2726 on 2025-01-24 08:46_

The current flag conflicts in groups are inconsistent (for example, `--only-dev` should conflict with `--group`, but it doesn't). I'd like to address these in a separate PR.

---

_@j178 reviewed on 2025-01-24 08:47_

---

_Review comment by @j178 on `crates/uv-configuration/src/dev.rs`:35 on 2025-01-24 08:47_

This method is not used anywhere.

---

_Review comment by @j178 on `crates/uv-configuration/src/dev.rs`:142 on 2025-01-24 08:49_

The `is_default_group: bool` argument is not ideal, but it seems to be the simplest solution I could come up with.

---

_@j178 reviewed on 2025-01-24 08:49_

---

_Marked ready for review by @j178 on 2025-01-24 08:50_

---

_Review requested from @Gankra by @Gankra on 2025-01-24 15:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-25 21:23_

---

_Closed by @Gankra on 2025-02-05 20:31_

---

_Closed by @Gankra on 2025-02-05 20:31_

---

_Comment by @Gankra on 2025-02-05 20:33_

Thanks for the PR! I agreed the code was hard to augment, so I overhauled the subsystem and landed fixes for these issues in #11224

---
