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
updated_at: 2026-01-14T09:22:09Z
url: https://github.com/astral-sh/uv/pull/17440
synced_at: 2026-01-14T09:35:22Z
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
