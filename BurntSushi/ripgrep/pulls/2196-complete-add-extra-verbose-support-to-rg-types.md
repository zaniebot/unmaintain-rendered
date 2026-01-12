```yaml
number: 2196
title: "complete: add extra-verbose support to _rg_types"
type: pull_request
state: closed
author: okdana
labels:
  - rollup
assignees: []
base: master
head: dana/2195
created_at: 2022-04-27T02:27:57Z
updated_at: 2023-07-08T22:54:11Z
url: https://github.com/BurntSushi/ripgrep/pull/2196
synced_at: 2026-01-12T18:23:14Z
```

# complete: add extra-verbose support to _rg_types

---

_@okdana_

When the `extra-verbose` style is set for the `types` tag, completed types are displayed along with the patterns they correspond to. This can be enabled by e.g. adding the following to `.zshrc`:

```
zstyle ':completion:*:rg:*:types' extra-verbose true
```

This change also makes `_rg_types` use the actual `rg` specified on the command line to look up types, and it fixes a mangled `complete-all` style check

Fixes #2195

---

_Comment by @Freed-Wu on 2022-10-15 19:05_

Any progress about this PR?

---

_@BurntSushi approved on 2023-07-08 13:50_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 13:51_

---

_Comment by @BurntSushi on 2023-07-08 22:54_

Closed by #2554 

---

_Closed by @BurntSushi on 2023-07-08 22:54_

---
