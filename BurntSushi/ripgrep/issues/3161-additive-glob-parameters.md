```yaml
number: 3161
title: additive glob parameters
type: issue
state: closed
author: mcandre
labels:
  - invalid
assignees: []
created_at: 2025-09-25T00:08:05Z
updated_at: 2025-09-25T00:21:24Z
url: https://github.com/BurntSushi/ripgrep/issues/3161
synced_at: 2026-01-12T16:13:25Z
```

# additive glob parameters

---

_@mcandre_

Please append rather than replace `--glob`... flags. For example, in `~/.ripgreprc`. ripgrep currently applies only the last --glob flag, which is unintuitive.

I beileve comma separated values may work around this. However, I need to configure very many exclusion patterns, as I work with very many tech stacks. Maintaining comma separated lists introduces complexity for the user, compared to supplying multiple --glob flags.

---

_Comment by @BurntSushi on 2025-09-25 00:10_

ripgrep does append here. Please provide an MRE.

---

_Comment by @mcandre on 2025-09-25 00:20_

Update:

I identified that ripgrep was completely ignoring my ~/.ripgreprc file.

As documented, ripgrep fails to look for or load its own configuration file.

This significantly departs with software conventions.

---

_Closed by @BurntSushi on 2025-09-25 00:21_

---

_Label `invalid` added by @BurntSushi on 2025-09-25 00:21_

---
