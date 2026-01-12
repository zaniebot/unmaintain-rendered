```yaml
number: 966
title: "Feature request: -I (--no-ignore) argument"
type: issue
state: closed
author: kbd
labels:
  - question
assignees: []
created_at: 2018-06-25T23:15:50Z
updated_at: 2018-07-02T17:16:49Z
url: https://github.com/BurntSushi/ripgrep/issues/966
synced_at: 2026-01-12T16:13:22Z
```

# Feature request: -I (--no-ignore) argument

---

_@kbd_

From [fd](https://github.com/sharkdp/fd) I'm used to typing `-H` to search hidden files or `-I` to not respect `.gitignore`. They also form a convenient mnemonic, `-HI`, to search everything.

Unfortunately `-H` is already in-use, but since `-I` isn't currently used by ripgrep I thought I'd ask if you'd want to add it as an alias for `--no-ignore`. Thanks.

---

_Comment by @BurntSushi on 2018-06-25 23:21_

Have you read [the guide's section on automatic filtering](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering)?

Namely, ripgrep already has an alias. `-u` stops respecting `.gitignore`. Specifying it again, `-uu`, will cause ripgrep to search hidden files. Specifying it a third time, `-uuu`, will cause ripgrep to search binary files.

---

_Comment by @kbd on 2018-06-25 23:29_

I did notice in the manpage that `-u` is equivalent to `-I` and `-uu` is equivalent to `-HI` as described above. I thought it could still be worth adding, but I understand if you don't want two single-letter aliases that do the same thing.

---

_Comment by @BurntSushi on 2018-06-25 23:53_

You got it. Single letter aliases are a limited resource, and `-u` is good enough.

---

_Closed by @BurntSushi on 2018-06-25 23:53_

---

_Label `question` added by @BurntSushi on 2018-06-25 23:54_

---

_Comment by @sharkdp on 2018-07-02 17:16_

@kbd It's a hidden feature, but (for consistency/symmetry reasons) you can use ripgrep's `-u` and `-uu` in `fd` as well.

---
