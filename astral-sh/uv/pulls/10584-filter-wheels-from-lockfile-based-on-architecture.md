```yaml
number: 10584
title: Filter wheels from lockfile based on architecture
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/arm-tags
created_at: 2025-01-14T03:27:19Z
updated_at: 2025-01-14T14:39:22Z
url: https://github.com/astral-sh/uv/pull/10584
synced_at: 2026-01-12T16:09:22Z
```

# Filter wheels from lockfile based on architecture

---

_@charliermarsh_

## Summary

After we resolve, we filter out any wheels that aren't applicable for the target platforms. So, e.g., we remove macOS wheels if we find that the user only asked to solve for Windows.

This PR extends the same logic to architectures, so that we filter out ARM-only wheels when the user is only solving for x86, etc.

Closes #10571.

---

_Review requested from @konstin by @charliermarsh on 2025-01-14 03:27_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:90 on 2025-01-14 03:27_

There... might be other markers to consider here?

---

_@charliermarsh reviewed on 2025-01-14 03:27_

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-14 03:28_

---

_Review request for @BurntSushi removed by @charliermarsh on 2025-01-14 03:28_

---

_Review requested from @Gankra by @charliermarsh on 2025-01-14 03:28_

---

_Label `enhancement` added by @charliermarsh on 2025-01-14 03:28_

---

_@charliermarsh reviewed on 2025-01-14 03:31_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform_tag.rs`:76 on 2025-01-14 03:31_

I renamed these, since, e.g., the `Self::Any` tag is also compatible, but it doesn't get at what we want here.

---

_@konstin approved on 2025-01-14 08:11_

Do we have or can we add a test that we don't remove wheel from platforms where we don't parse the architecture e.g. a freebsd x86_64 wheel under `platform_machine == 'x86_64'`?

---

_Comment by @charliermarsh on 2025-01-14 13:45_

I guess that would have to go through packse? (Not sure where else to get a FreeBSD wheel.)

---

_@BurntSushi approved on 2025-01-14 13:46_

---

_Comment by @charliermarsh on 2025-01-14 14:28_

Added.

---

_Merged by @charliermarsh on 2025-01-14 14:39_

---

_Closed by @charliermarsh on 2025-01-14 14:39_

---

_Branch deleted on 2025-01-14 14:39_

---
