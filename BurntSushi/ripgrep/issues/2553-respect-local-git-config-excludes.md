```yaml
number: 2553
title: "respect local `.git/config` excludes"
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
  - gitignore
assignees: []
created_at: 2023-07-07T18:35:10Z
updated_at: 2023-07-07T18:35:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2553
synced_at: 2026-01-12T16:13:24Z
```

# respect local `.git/config` excludes

---

_@BurntSushi_

Copied from: https://github.com/BurntSushi/ripgrep/pull/2396

Currently, ripgrep respects `$HOME/.gitconfig` and `$XDG_CONFIG_HOME/git/config`, but not `.git/config` (the "local" or "project" config). Git makes it such that `.git/config` overrides the other two options, while still respecting the `./.gitignore`. This commit brings this functionality to ripgrep (while also cleaning a few repetitive segments of code).

---

_Label `enhancement` added by @BurntSushi on 2023-07-07 18:35_

---

_Label `gitignore` added by @BurntSushi on 2023-07-07 18:35_

---
