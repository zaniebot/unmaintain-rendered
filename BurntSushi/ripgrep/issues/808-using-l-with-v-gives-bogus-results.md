```yaml
number: 808
title: Using -l with -v gives bogus results
type: issue
state: closed
author: insidewhy
labels:
  - invalid
assignees: []
created_at: 2018-02-15T15:17:53Z
updated_at: 2018-02-15T15:36:12Z
url: https://github.com/BurntSushi/ripgrep/issues/808
synced_at: 2026-01-12T16:13:22Z
```

# Using -l with -v gives bogus results

---

_@insidewhy_

#### What version of ripgrep are you using?

ripgrep 0.7.1
-AVX -SIMD

#### What operating system are you using ripgrep on?

Archlinux

#### Describe your question, feature request, or bug.

bug

#### If this is a bug, what are the steps to reproduce the behavior?

Run this in a git repo containing a bunch of files, some of which contain the string `match` and some of which don't.

```
rg -l -v match
```

#### If this is a bug, what is the actual behavior?

I get a bunch of files listed that contain the string `match`.

#### If this is a bug, what is the expected behavior?

The listed files should not contain the string `match` (this is how `grep -l -v match **/*` works).

---

_Comment by @insidewhy on 2018-02-15 15:21_

Hm I was wrong about grep, I guess it makes sense what with it being line based, but it's kinda useless ah well.

---

_Closed by @insidewhy on 2018-02-15 15:21_

---

_Comment by @BurntSushi on 2018-02-15 15:36_

Indeed. I suspect the flag you're looking for is `--files-without-match`.

---

_Label `invalid` added by @BurntSushi on 2018-02-15 15:36_

---
