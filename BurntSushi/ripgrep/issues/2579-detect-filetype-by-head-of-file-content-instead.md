```yaml
number: 2579
title: Detect filetype by head of file content instead of extension name
type: issue
state: closed
author: spin6lock
labels:
  - wontfix
assignees: []
created_at: 2023-08-05T13:06:51Z
updated_at: 2023-08-07T04:24:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2579
synced_at: 2026-01-12T16:13:24Z
```

# Detect filetype by head of file content instead of extension name

---

_@spin6lock_

#### auto detect filetype by head of the file

ripgrep detect compress file type by extension name. It would be nice that support filetype detection by head of file. Then we can search zst file by not only naming *.zst, but also *.zst.1, *.zst.2 etc. Such as this in [zgrep test_format function](https://fossies.org/linux/zutils/zutils.cc).

The usage is the same as old one, no changes in manpage



---

_Comment by @BurntSushi on 2023-08-05 13:30_

I don't have any plans to implement this, sorry. There is a big difference between detecting file type by looking at the name and actually opening the file and reading its contents. The latter requires a lot more work.

---

_Closed by @BurntSushi on 2023-08-05 13:30_

---

_Label `wontfix` added by @BurntSushi on 2023-08-05 13:31_

---

_Comment by @spin6lock on 2023-08-07 04:24_

ok, thanks for sharing your thought

---
