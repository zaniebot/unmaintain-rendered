```yaml
number: 14076
title: "Fix handling of changes to `requires-python`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: ag/empty-lock-file
created_at: 2025-06-16T14:50:51Z
updated_at: 2025-06-17T15:50:07Z
url: https://github.com/astral-sh/uv/pull/14076
synced_at: 2026-01-12T16:11:01Z
```

# Fix handling of changes to `requires-python`

---

_@BurntSushi_

When using `uv lock --upgrade-package=python` after changing
`requires-python`, it was possible to get into a state where the fork
markers produced corresponded to the empty set. This in turn resulted in
an empty lock file.

There was already some infrastructure in place that I think was perhaps
intended to handle this. In particular, `Lock::check_marker_coverage`
checks whether the fork markers have some overlap with the supported
environments (including the `requires-python`). But there were two
problems with this.

First is that in lock validation, this marker coverage check came
_after_ a path that returned `Preferable` (meaning that the fork markers
should be kept) when `--upgrade-package` was used. Second is that the
marker coverage check used the `requires-python` in the lock file and
_not_ the `requires-python` in the now updated `pyproject.toml`.

We attempt to solve this conundrum by slightly re-arranging lock file
validation and by explicitly checking whether the *new*
`requires-python` is disjoint from the fork markers in the lock file. If
it is, then we return `Versions` from lock file validation (indicating
that the fork markers should be dropped).

Fixes #13951


---

_Label `bug` added by @BurntSushi on 2025-06-16 14:51_

---

_Label `lock` added by @BurntSushi on 2025-06-16 14:51_

---

_Review requested from @charliermarsh by @BurntSushi on 2025-06-16 14:51_

---

_Review requested from @konstin by @BurntSushi on 2025-06-16 14:51_

---

_@konstin approved on 2025-06-17 10:40_

---

_@charliermarsh reviewed on 2025-06-17 11:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:999 on 2025-06-17 11:49_

Hmm, are you sure this should be a user-facing warning? I don’t think any of the other lockfile-invalidation messages are user-facing. They just go through tracing.

---

_@BurntSushi reviewed on 2025-06-17 11:53_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:999 on 2025-06-17 11:53_

Oh, I didn't do that. That's what is on `main`: https://github.com/astral-sh/uv/blob/5c1ebf902bad2ed8058b57deaca9645de1286eca/crates/uv/src/commands/project/lock.rs#L1026-L1034

It looks like it was a user facing warning from the beginning: https://github.com/astral-sh/uv/pull/10682

I in turn copied this to the disjoint `requires-python` check as well. Do you want me to make both of them traces instead?

---

_@charliermarsh reviewed on 2025-06-17 13:54_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:999 on 2025-06-17 13:54_

Ah ok. Just leave it as-is then, thanks.

---

_Merged by @BurntSushi on 2025-06-17 15:50_

---

_Closed by @BurntSushi on 2025-06-17 15:50_

---

_Branch deleted on 2025-06-17 15:50_

---
