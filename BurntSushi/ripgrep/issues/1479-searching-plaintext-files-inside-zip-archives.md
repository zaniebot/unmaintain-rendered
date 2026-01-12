```yaml
number: 1479
title: searching plaintext files inside zip archives
type: issue
state: closed
author: eadmaster
labels:
  - duplicate
assignees: []
created_at: 2020-02-07T20:46:28Z
updated_at: 2020-02-07T23:48:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1479
synced_at: 2026-01-12T16:13:23Z
```

# searching plaintext files inside zip archives

---

_@eadmaster_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

https://github.com/BurntSushi/ripgrep/releases/download/11.0.2/ripgrep-11.0.2-x86_64-unknown-linux-musl.tar.gz

#### What operating system are you using ripgrep on?

  Operating System: Ubuntu 18.04.2 LTS
            Kernel: Linux 4.15.0-50-generic
      Architecture: x86-64


#### Describe your question, feature request, or bug.



#### If this is a bug, what are the steps to reproduce the behavior?

`rg --search-zip "Super Metroid"  "$PENDRIVE/Documents/db/datfile/No Intro DAT-o-MATIC (2016-01-03).zip"`

#### If this is a bug, what is the actual behavior?

(no output)

#### If this is a bug, what is the expected behavior?

Should look for matches inside the zip archive



---

_Label `duplicate` added by @BurntSushi on 2020-02-07 23:48_

---

_Comment by @BurntSushi on 2020-02-07 23:48_

Duplicate of https://github.com/BurntSushi/ripgrep/issues/1112

---

_Closed by @BurntSushi on 2020-02-07 23:48_

---
