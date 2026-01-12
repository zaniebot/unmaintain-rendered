```yaml
number: 14261
title: Ensure preview default Python installs are upgradeable
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: jtfm/upgrade-default
created_at: 2025-06-25T14:10:53Z
updated_at: 2025-06-27T17:26:30Z
url: https://github.com/astral-sh/uv/pull/14261
synced_at: 2026-01-12T16:11:07Z
```

# Ensure preview default Python installs are upgradeable

---

_@jtfmumm_

Python `bin` installations installed with `uv python install --default --preview` (no version specified) were not being installed as upgradeable. Instead each link was pointed at the highest patch version for a minor version. This change ensures that these preview default installations are also treated as upgradeable.

The PR includes some updates to the related tests. First, it checks the default install without specified version case. Second, since it's adding more read link checks, it creates a new `read_link` helper method to consolidate repeated logic and replace instances of `#[cfg(unix/windows)` with `if cfg!(unix/windows)`.

Fixes #14247


---

_Label `bug` added by @jtfmumm on 2025-06-25 14:10_

---

_@jtfmumm reviewed on 2025-06-25 14:11_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:454 on 2025-06-25 14:11_

I removed the `preview.is_enabled()` check since it's redundant (`preview` is used in `create_bin_links` to gate symlink directory creation) and makes this predicate harder to read with this change

---

_@jtfmumm reviewed on 2025-06-25 14:12_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:454 on 2025-06-25 14:12_

Note that this is the core change here. The rest of this PR consists of test updates.

---

_Review requested from @zanieb by @jtfmumm on 2025-06-27 15:33_

---

_@zanieb approved on 2025-06-27 17:24_

---

_Label `preview` added by @zanieb on 2025-06-27 17:25_

---

_Comment by @zanieb on 2025-06-27 17:25_

Remember to use the preview label so they're grouped properly.

---

_Merged by @jtfmumm on 2025-06-27 17:26_

---

_Closed by @jtfmumm on 2025-06-27 17:26_

---

_Branch deleted on 2025-06-27 17:26_

---
