```yaml
number: 2333
title: "`-c` (`--count`) does not count multiple times in same line"
type: issue
state: closed
author: nopeless
labels:
  - invalid
assignees: []
created_at: 2022-10-17T20:09:59Z
updated_at: 2022-10-17T20:48:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2333
synced_at: 2026-01-12T16:13:24Z
```

# `-c` (`--count`) does not count multiple times in same line

---

_@nopeless_

I am sure that this is intended behavior

However, I would like an option to count multiple times in a single line. If there is already a way to do this (thats not written in the docs) let me know

---

_Comment by @BurntSushi on 2022-10-17 20:48_

This is correct, documented, intended and consistent with grep. Moreover, the documentation for `--count` answers your question: https://github.com/BurntSushi/ripgrep/blob/4386b8e805e273a9795ad4e256c455c74407c949/crates/core/app.rs#L1057

Use `--count-matches` or `-oc`.

---

_Closed by @BurntSushi on 2022-10-17 20:48_

---

_Label `invalid` added by @BurntSushi on 2022-10-17 20:48_

---
