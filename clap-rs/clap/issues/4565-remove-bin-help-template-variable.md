```yaml
number: 4565
title: "Remove `{bin}` help template variable"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-help
  - E-easy
assignees: []
created_at: 2022-12-20T13:07:48Z
updated_at: 2022-12-22T16:33:24Z
url: https://github.com/clap-rs/clap/issues/4565
synced_at: 2026-01-12T16:14:16Z
```

# Remove `{bin}` help template variable

---

_@epage_

When we added the `{name}` variable, we forgot to remove the broken `{bin}` variable in 4.0 and people are still accidentally using it for new use cases (#4561).

When removing it, we should assert on `{bin}` not being in the help template to help people transition across the breaking change.

---

_Label `C-bug` added by @epage on 2022-12-20 13:07_

---

_Label `M-breaking-change` added by @epage on 2022-12-20 13:07_

---

_Label `A-help` added by @epage on 2022-12-20 13:07_

---

_Label `E-easy` added by @epage on 2022-12-20 13:07_

---

_Added to milestone `5.0` by @epage on 2022-12-20 13:07_

---

_Referenced in [clap-rs/clap#4566](../../clap-rs/clap/pulls/4566.md) on 2022-12-21 00:59_

---

_Closed by @epage on 2022-12-22 16:33_

---
