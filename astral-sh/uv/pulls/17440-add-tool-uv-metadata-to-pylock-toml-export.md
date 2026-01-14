```yaml
number: 17440
title: "Add [tool.uv] metadata to pylock.toml export"
type: pull_request
state: open
author: gaborbernat
labels: []
assignees: []
draft: true
base: main
head: pylock-tool-uv
created_at: 2026-01-13T16:20:10Z
updated_at: 2026-01-14T12:59:14Z
url: https://github.com/astral-sh/uv/pull/17440
synced_at: 2026-01-14T13:42:35Z
```

# Add [tool.uv] metadata to pylock.toml export

---

_@gaborbernat_

Add tool.uv section to PEP 751 pylock.toml output containing:
- version: Current uv version for reproducibility tracking
- command: Command line args used to generate the lock file


---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:978 on 2026-01-13 21:17_

This should use `uv-version::version`

---

_@zanieb reviewed on 2026-01-13 21:17_

---

_@zanieb reviewed on 2026-01-13 21:17_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-13 21:17_

What's happening here? Stripping the executable path?

---

_Review comment by @gaborbernat on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-13 21:27_

Yes 🤔

---

_@gaborbernat reviewed on 2026-01-13 21:27_

---

_@zanieb reviewed on 2026-01-13 21:33_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-13 21:33_

Is that usually present? Want to add a comment explaining what is happening and why?

---

_@gaborbernat reviewed on 2026-01-13 21:35_

---

_Review comment by @gaborbernat on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-13 21:35_

Yes I will do so however because this is dependent on my other PR I will do it once that lands. I actually initially added it there but then I was asked to split it out at the separate feature so open it here and mark it as draft until that one lands. 

---

_@konstin reviewed on 2026-01-14 09:22_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-14 09:22_

How is that PR dependent on the other PR?

---

_@gaborbernat reviewed on 2026-01-14 12:52_

---

_Review comment by @gaborbernat on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-14 12:52_

The other one adds many tests that needs updating once it lands with contents of this. 

---

_@gaborbernat reviewed on 2026-01-14 12:52_

---

_Review comment by @gaborbernat on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-14 12:52_

The other one adds many tests that needs updating once it lands with contents of this. 

---

_@konstin reviewed on 2026-01-14 12:57_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-14 12:57_

I'm fine with waiting, but fwiw rebasing and `cargo insta test --accept` usually work well.

---

_@gaborbernat reviewed on 2026-01-14 12:59_

---

_Review comment by @gaborbernat on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:985 on 2026-01-14 12:59_

Yes but means there's some work that needs to be done, hence depends. 

---
