```yaml
number: 4940
title: "Improve 'any' search message during `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/install-search
created_at: 2024-07-09T19:44:35Z
updated_at: 2024-07-10T15:03:23Z
url: https://github.com/astral-sh/uv/pull/4940
synced_at: 2026-01-12T16:06:32Z
```

# Improve 'any' search message during `uv python install`

---

_@zanieb_

Special cases the `Any` request in output

e.g.,

```
❯ cargo run -q -- python install --isolated
warning: `uv python install` is experimental and may change without warning.
Searching for Python installations
Found existing installation: cpython-3.12.3-macos-aarch64-none
Python is already available. Use `uv python install <request>` to install a specific version.
```

instead of 

```
❯ cargo run -q -- python install --isolated
warning: `uv python install` is experimental and may change without warning.
Searching for Python versions matching: any Python
Found existing installation for any Python: cpython-3.12.3-macos-aarch64-none
Python is already available. Use `uv python install <request>` to install a specific version.
```

---

_Label `cli` added by @zanieb on 2024-07-09 19:44_

---

_Renamed from "Improve any search message during `uv python install`" to "Improve 'any' search message during `uv python install`" by @zanieb on 2024-07-09 19:46_

---

_Comment by @zanieb on 2024-07-09 19:47_

Loosely considering keeping the existing messages when `--verbose` is used.

---

_Label `preview` added by @zanieb on 2024-07-09 19:47_

---

_@charliermarsh reviewed on 2024-07-09 20:57_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:80 on 2024-07-09 20:57_

I think in `uninstall` I just made this `"Found: {}"` since we don't need to show the request.

---

_@charliermarsh approved on 2024-07-09 20:57_

---

_Merged by @zanieb on 2024-07-10 15:03_

---

_Closed by @zanieb on 2024-07-10 15:03_

---

_Branch deleted on 2024-07-10 15:03_

---
