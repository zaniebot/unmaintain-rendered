```yaml
number: 3986
title: "uv {lock,sync}: propagate index URLs to registry client"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/index-urls-in-client
created_at: 2024-06-03T14:30:29Z
updated_at: 2024-06-03T14:38:37Z
url: https://github.com/astral-sh/uv/pull/3986
synced_at: 2026-01-10T13:59:34Z
```

# uv {lock,sync}: propagate index URLs to registry client

---

_Pull request opened by @BurntSushi on 2024-06-03 14:30_

Otherwise the `uv lock` command wasn't respecting the index URL option.

This is a follow-up to #3984, and I believe should now allow #3970 to be
merged.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-06-03 14:30_

---

_@charliermarsh reviewed on 2024-06-03 14:31_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:39 on 2024-06-03 14:31_

Nit: remove

---

_@charliermarsh approved on 2024-06-03 14:31_

---

_Comment by @charliermarsh on 2024-06-03 14:31_

Can you also change `update_environment` to do this?

---

_Comment by @BurntSushi on 2024-06-03 14:32_

> Can you also change `update_environment` to do this?

Nice catch. Done.

---

_Merged by @BurntSushi on 2024-06-03 14:38_

---

_Closed by @BurntSushi on 2024-06-03 14:38_

---

_Branch deleted on 2024-06-03 14:38_

---
