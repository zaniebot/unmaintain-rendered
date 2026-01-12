```yaml
number: 2631
title: "WalkBuilder::filter_entry() is weirdly restrictive"
type: issue
state: closed
author: matthiasbeyer
labels: []
assignees: []
created_at: 2023-10-15T13:32:09Z
updated_at: 2023-10-15T13:39:22Z
url: https://github.com/BurntSushi/ripgrep/issues/2631
synced_at: 2026-01-12T16:13:24Z
```

# WalkBuilder::filter_entry() is weirdly restrictive

---

_@matthiasbeyer_

https://github.com/BurntSushi/ripgrep/blob/7099e174acbcbd940f57e4ab4913fee4040c826e/crates/ignore/src/walk.rs#L899

This argument restriction seems weirdly restrictive to me. Is there any particular reason why the closure one has to pass in has to be a pure function (`Fn + 'static`)? Any chance that these might get lifted?

---

_Locked by @ghost on 2023-10-15 13:39_

---
