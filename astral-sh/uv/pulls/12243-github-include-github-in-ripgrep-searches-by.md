```yaml
number: 12243
title: "github: include /.github/ in ripgrep searches by default"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/allowlist-github
created_at: 2025-03-17T16:39:10Z
updated_at: 2025-03-17T17:13:51Z
url: https://github.com/astral-sh/uv/pull/12243
synced_at: 2026-01-12T16:10:11Z
```

# github: include /.github/ in ripgrep searches by default

---

_@BurntSushi_

Previously, unless you had some other configuration that impacts
ripgrep, `rg -tyaml uses:` would return zero results. After this
changes, it returns more of what you might expect.

This is because ripgrep ignores hidden files and directories by default.
But arguably, searching `.github` by default is probably what we want.

I do the same thing in ripgrep's repository:
https://github.com/BurntSushi/ripgrep/blob/de4baa10024f2cb62d438596274b9b710e01c59b/.ignore#L1

Companion PR: https://github.com/astral-sh/ruff/pull/16814


---

_Review requested from @zanieb by @BurntSushi on 2025-03-17 16:39_

---

_@zanieb approved on 2025-03-17 17:13_

---

_Merged by @zanieb on 2025-03-17 17:13_

---

_Closed by @zanieb on 2025-03-17 17:13_

---

_Branch deleted on 2025-03-17 17:13_

---
