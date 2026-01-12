```yaml
number: 16814
title: "github: include `/.github/` in ripgrep searches by default"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/allowlist-github
created_at: 2025-03-17T15:37:07Z
updated_at: 2025-03-17T16:37:59Z
url: https://github.com/astral-sh/ruff/pull/16814
synced_at: 2026-01-12T15:55:58Z
```

# github: include `/.github/` in ripgrep searches by default

---

_@BurntSushi_

Previously, unless you had some other configuration that impacts
ripgrep, `rg -tyaml uses:` would return zero results. After this
changes, it returns more of what you might expect.

This is because ripgrep ignores hidden files and directories by default.
But arguably, searching `.github` by default is probably what we want.

I do the same thing in ripgrep's repository:
https://github.com/BurntSushi/ripgrep/blob/de4baa10024f2cb62d438596274b9b710e01c59b/.ignore#L1


---

_Label `internal` added by @MichaReiser on 2025-03-17 16:26_

---

_@MichaReiser approved on 2025-03-17 16:26_

---

_Merged by @BurntSushi on 2025-03-17 16:37_

---

_Closed by @BurntSushi on 2025-03-17 16:37_

---

_Branch deleted on 2025-03-17 16:37_

---
