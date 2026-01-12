```yaml
number: 2672
title: Binaries for Arm64 Windows
type: issue
state: closed
author: Xazax-hun
labels:
  - wontfix
assignees: []
created_at: 2023-12-03T19:08:03Z
updated_at: 2023-12-03T19:12:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2672
synced_at: 2026-01-12T16:13:24Z
```

# Binaries for Arm64 Windows

---

_@Xazax-hun_

It would be nice to release binaries for Windows on Arm64.

---

_Comment by @BurntSushi on 2023-12-03 19:12_

Not happening, sorry.

1. Not supported in GitHub Actions: https://github.com/actions/runner-images/issues/768
2. Not supported in Rust's de facto standard cross compilation tool: https://github.com/cross-rs/cross
3. And I have zero plans to purchase a Windows arm64 machine.

If you want this, you'll want to make (1) or (2) happen first. Then you can come back here and re-open this issue.

---

_Closed by @BurntSushi on 2023-12-03 19:12_

---

_Label `wontfix` added by @BurntSushi on 2023-12-03 19:12_

---
