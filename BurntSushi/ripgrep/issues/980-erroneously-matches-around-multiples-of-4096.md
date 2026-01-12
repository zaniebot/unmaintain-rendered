```yaml
number: 980
title: ^$ erroneously matches around multiples of 4096
type: issue
state: closed
author: jleedev
labels:
  - bug
  - libripgrep
assignees: []
created_at: 2018-07-13T14:21:51Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/980
synced_at: 2026-01-12T16:13:22Z
```

# ^$ erroneously matches around multiples of 4096

---

_@jleedev_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

cargo install
rustc 1.27.0 (3eda71b00 2018-06-19)

#### What operating system are you using ripgrep on?

`Linux 3.13.0-43-generic #72-Ubuntu SMP Mon Dec 8 19:35:06 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux`
`Linux 4.14.34-v7+ #1110 SMP Mon Apr 16 15:18:51 BST 2018 armv7l GNU/Linux`
`Darwin 17.7.0 Darwin Kernel Version 17.7.0: Thu Jun 21 22:53:14 PDT 2018; root:xnu-4570.71.2~1/RELEASE_X86_64 x86_64`

#### Describe your question, feature request, or bug.

```
~$ yes | rg -n ^$ | head 
2:
4098:
8195:
12292:
16389:
20486:
24583:
28680:
32777:
36874:
```

#### If this is a bug, what is the expected behavior?

`yes | rg ^$` should not find any matches.

This also seems to break line numbers on any pattern that could possibly match a blank line.

(I was searching for `^.{0,12}$` in a file that contains no empty lines and was surprised to find empty lines. My workaround was to change the pattern to `^{1,12}$`, but I was looking at incorrect line numbers until then.)

```
~$ yes | rg -C1 -n ^$ | head 
1-y
2:
3-y
--
4097-y
4098:
4099-y
--
8194-y
8195:
```

This looks similar to #441, but that's only at the end of file.

---

_Comment by @BurntSushi on 2018-07-13 14:38_

@jleedev Great bug. I suspect this is actually the similar or same bug as #441. The tipoff here is the multiple size, which seems close to lining up with the internal buffer size used by ripgrep, which probably triggers a similar condition as being at the end of a file.

This _should_ be fixed in libripgrep. I'll confirm this bug and add a regression test there.

---

_Label `bug` added by @BurntSushi on 2018-07-13 14:38_

---

_Label `libripgrep` added by @BurntSushi on 2018-07-13 14:38_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-07-13 14:38_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
