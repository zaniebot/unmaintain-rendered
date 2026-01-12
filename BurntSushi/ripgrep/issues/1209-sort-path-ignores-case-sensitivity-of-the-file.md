```yaml
number: 1209
title: "--sort path ignores case sensitivity of the file system"
type: issue
state: closed
author: laggardkernel
labels:
  - bug
  - wontfix
assignees: []
created_at: 2019-03-04T18:09:46Z
updated_at: 2019-03-04T18:16:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1209
synced_at: 2026-01-12T16:13:23Z
```

# --sort path ignores case sensitivity of the file system

---

_@laggardkernel_

#### What version of ripgrep are you using?

```bash
❯ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Installed on macOS 10.12.6 with homebrew.

#### Describe your question, feature request, or bug.

The sort option `--sort path` sort the result by their file path. But it doesn't respect the case sensitivity of the file system. The result is sorted case-sensitively.


---

_Comment by @BurntSushi on 2019-03-04 18:15_

AFAIK, the correct way to implement this is to use an implementation of the [Unicode collation algorithm](https://www.unicode.org/reports/tr10/tr10-38.html). A high quality implementation of this does not exist in Rust yet. When it does, we can revisit this. Until then, I'm going to unfortunately mark this as `wontfix`. Sorry.

---

_Label `bug` added by @BurntSushi on 2019-03-04 18:15_

---

_Label `wontfix` added by @BurntSushi on 2019-03-04 18:15_

---

_Closed by @BurntSushi on 2019-03-04 18:15_

---
