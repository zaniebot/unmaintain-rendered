```yaml
number: 2482
title: "Make gitignore.rs's `gitconfig_excludes_path` public"
type: pull_request
state: closed
author: Eric-Arellano
labels:
  - rollup
assignees: []
base: master
head: global-gitignore-helper-is-public
created_at: 2023-04-01T19:14:54Z
updated_at: 2023-07-09T14:45:42Z
url: https://github.com/BurntSushi/ripgrep/pull/2482
synced_at: 2026-01-12T18:23:14Z
```

# Make gitignore.rs's `gitconfig_excludes_path` public

---

_@Eric-Arellano_

In Pantsbuild, we use `GitignoreBuilder` to determine which files our file watcher should look at.

We want to improve the file watcher to consider the global gitignore file. It would be exceptionally useful if we could reuse your logic: https://github.com/pantsbuild/pants/pull/18649

P.S. Thanks for ripgrep! You've saved me so much time 🙌

---

_Label `rollup` added by @BurntSushi on 2023-07-07 14:35_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

_Branch deleted on 2023-07-09 14:45_

---

_Comment by @Eric-Arellano on 2023-07-09 14:45_

Thank you!

---
