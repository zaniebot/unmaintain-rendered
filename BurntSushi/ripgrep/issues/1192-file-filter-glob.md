```yaml
number: 1192
title: File filter/glob?
type: issue
state: closed
author: alexreg
labels:
  - invalid
assignees: []
created_at: 2019-02-09T21:14:15Z
updated_at: 2019-02-09T21:19:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1192
synced_at: 2026-01-12T16:13:23Z
```

# File filter/glob?

---

_@alexreg_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

macOS Homebrew

#### What operating system are you using ripgrep on?

macOS 10.14.2

#### Describe your question, feature request, or bug.

Could we have a feature to search files that match a certain glob/pattern, so the expansion doesn't have to take place in the shell (which is slow, and sometimes too large)? E.g., similar to the `-name` option of find.

---

_Comment by @BurntSushi on 2019-02-09 21:19_

[Please consult the documentation.](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs)

---

_Closed by @BurntSushi on 2019-02-09 21:19_

---

_Label `invalid` added by @BurntSushi on 2019-02-09 21:19_

---
