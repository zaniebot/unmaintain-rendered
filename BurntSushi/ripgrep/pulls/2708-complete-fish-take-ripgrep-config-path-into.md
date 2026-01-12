```yaml
number: 2708
title: "complete/fish: Take RIPGREP_CONFIG_PATH into account"
type: pull_request
state: closed
author: blyxxyz
labels:
  - rollup
assignees: []
base: master
head: fish-complete-read-config
created_at: 2024-01-10T10:38:22Z
updated_at: 2025-09-20T01:08:21Z
url: https://github.com/BurntSushi/ripgrep/pull/2708
synced_at: 2026-01-12T18:23:14Z
```

# complete/fish: Take RIPGREP_CONFIG_PATH into account

---

_@blyxxyz_

The fish completions now also pay attention to the configuration file to determine whether to suggest negation options and not just to the current command line.

This doesn't cover all edge cases. For example the config file is cached, and so changes may not take effect until the next shell session. But the cases it doesn't cover are hopefully very rare.

See the post hoc discussion on https://github.com/BurntSushi/ripgrep/pull/2684.

cc @cstyles 

---

_@BurntSushi approved on 2025-07-05 12:25_

Thank you!

---

_Label `rollup` added by @BurntSushi on 2025-07-05 12:27_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
