```yaml
number: 2552
title: a broken symlink results in ripgrep emitting an error even if the symlink is ignored
type: issue
state: open
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2023-07-07T18:16:40Z
updated_at: 2023-07-07T18:16:48Z
url: https://github.com/BurntSushi/ripgrep/issues/2552
synced_at: 2026-01-12T16:13:24Z
```

# a broken symlink results in ripgrep emitting an error even if the symlink is ignored

---

_@BurntSushi_

Copied from https://github.com/BurntSushi/ripgrep/pull/2431:

Steps to reproduce:

- Create parallel walker with "follow links" option enabled.
- Create broken symlink with the name broken_symlink, for example.
- Add "broken_symlink" to ignored files.
- Get walker dir entry results.

Expected: walker doesn't return dir entry result for the link.
Actual: walker returns error result for the link.

---

_Label `bug` added by @BurntSushi on 2023-07-07 18:16_

---
