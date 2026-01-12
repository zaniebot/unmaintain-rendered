```yaml
number: 2026
title: Allow filtering out binary files with --files option
type: issue
state: closed
author: Uroc327
labels:
  - wontfix
assignees: []
created_at: 2021-10-19T09:17:36Z
updated_at: 2021-10-19T11:49:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2026
synced_at: 2026-01-12T16:13:24Z
```

# Allow filtering out binary files with --files option

---

_@Uroc327_

When the `--files` option is used, it seems that scanning for and filtering out of binary files is disabled. I'd appreciate a way to (re-)enable this.

I'm unsure, if this is a bug or a currently unimplemented feature.

---

_Comment by @BurntSushi on 2021-10-19 11:11_

This is intended behavior because the only way to detect binary files is to read them. The `--files` flag specifically does not read files.

Probably you can get what you want with `rg -l '^'`.

---

_Closed by @BurntSushi on 2021-10-19 11:11_

---

_Label `wontfix` added by @BurntSushi on 2021-10-19 11:12_

---

_Comment by @Uroc327 on 2021-10-19 11:21_

In that case, would it be an option to implement an additional flag that makes `--files` actually read the files?

---

_Comment by @BurntSushi on 2021-10-19 11:49_

I don't think it's worth doing. Sorry.

---
