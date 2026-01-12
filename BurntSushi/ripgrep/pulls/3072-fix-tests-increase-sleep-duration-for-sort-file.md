```yaml
number: 3072
title: "fix(tests): Increase sleep duration for sort file metadata tests on Windows AArch64"
type: pull_request
state: closed
author: vishvanatarajan
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2025-06-21T20:16:42Z
updated_at: 2025-09-20T01:08:43Z
url: https://github.com/BurntSushi/ripgrep/pull/3072
synced_at: 2026-01-12T18:23:15Z
```

# fix(tests): Increase sleep duration for sort file metadata tests on Windows AArch64

---

_@vishvanatarajan_

Use cfg! to assign a 1000ms delay only on Windows Aarch64 targets. As it is a compile time conditional, there should be no runtime overhead.

Closes #3071

---

_Closed by @BurntSushi on 2025-08-19 21:16_

---

_Reopened by @BurntSushi on 2025-08-19 21:16_

---

_Label `rollup` added by @BurntSushi on 2025-08-19 21:21_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
