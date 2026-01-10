```yaml
number: 14108
title: "Don't use walrus operator in interpreter query script"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-use-walrus
created_at: 2025-06-17T15:51:57Z
updated_at: 2025-06-17T17:18:09Z
url: https://github.com/astral-sh/uv/pull/14108
synced_at: 2026-01-10T11:10:42Z
```

# Don't use walrus operator in interpreter query script

---

_Pull request opened by @konstin on 2025-06-17 15:51_

Fix `uv run -p 3.7` by not using a walrus operator. Python 3.7 isn't really supported anymore, but there's no reason to break interpreter discovery for it.

---

_Label `bug` added by @konstin on 2025-06-17 15:51_

---

_@Gankra approved on 2025-06-17 16:01_

Looks like ruff is catching a ton more fixes now

---

_Comment by @ntBre on 2025-06-17 16:11_

You can add something like this to your ruff config to avoid the errors if it's okay for the version to be higher:

```toml
[per-file-target-version]
"scripts/*.py" = "py312"
```

That should cover everything except the errors in `crates/uv-python/fetch-download-metadata.py`.

---

_@zanieb approved on 2025-06-17 16:15_

---

_Comment by @zanieb on 2025-06-17 16:25_

Note this regressed in https://github.com/astral-sh/uv/pull/11131 which was released in 0.7.9

---

_Merged by @zanieb on 2025-06-17 17:18_

---

_Closed by @zanieb on 2025-06-17 17:18_

---

_Branch deleted on 2025-06-17 17:18_

---
