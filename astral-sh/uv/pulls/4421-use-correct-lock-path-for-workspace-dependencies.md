```yaml
number: 4421
title: Use correct lock path for workspace dependencies
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/fix-workspace-path-lowering-edge-case
created_at: 2024-06-20T10:57:47Z
updated_at: 2024-06-20T13:28:48Z
url: https://github.com/astral-sh/uv/pull/4421
synced_at: 2026-01-10T13:54:02Z
```

# Use correct lock path for workspace dependencies

---

_Pull request opened by @konstin on 2024-06-20 10:57_

Previously, distributions created through `Source::Workspace` would have the absolute path as lock path. This didn't cause any problems, since in `Urls` we would later overwrite those urls with the correct one created from being workspace members by path.

Changing the order surfaced this. This change emits the correct lock path. I've manually checked the difference with `dbg!`, this is not observable on main, but on the diverging urls branch it fixes lockfile creation.

---

_Review requested from @BurntSushi by @konstin on 2024-06-20 10:57_

---

_@konstin reviewed on 2024-06-20 10:58_

---

_Review comment by @konstin on `crates/uv-resolver/src/manifest.rs`:103 on 2024-06-20 10:58_

No changes here, i just nudged rustfmt into right direction (i think it didn't like the comma placement)

---

_@BurntSushi approved on 2024-06-20 11:45_

---

_Merged by @konstin on 2024-06-20 13:28_

---

_Closed by @konstin on 2024-06-20 13:28_

---

_Branch deleted on 2024-06-20 13:28_

---
