```yaml
number: 810
title: Port to fancy-regex for look-around
type: issue
state: closed
author: dbkaplun
labels:
  - enhancement
  - question
assignees: []
created_at: 2018-02-16T13:32:30Z
updated_at: 2018-02-16T13:40:01Z
url: https://github.com/BurntSushi/ripgrep/issues/810
synced_at: 2026-01-12T16:13:22Z
```

# Port to fancy-regex for look-around

---

_@dbkaplun_

It would be great if ripgrep supported look-around. [fancy-regex](https://github.com/google/fancy-regex) does look-around. It is fairly new so let's investigate the possibility of replacing the NFA-based regex in ripgrep.

---

_Label `enhancement` added by @BurntSushi on 2018-02-16 13:39_

---

_Label `question` added by @BurntSushi on 2018-02-16 13:39_

---

_Label `wontfix` added by @BurntSushi on 2018-02-16 13:39_

---

_Label `wontfix` removed by @BurntSushi on 2018-02-16 13:39_

---

_Comment by @BurntSushi on 2018-02-16 13:40_

I'm well aware of `fancy-regex`. I think it's a great project. It needs to mature first before I would consider using it in ripgrep. It would also be behind an opt-in flag. See [the FAQ](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#fancy).

> NFA-based regex in ripgrep

This is wrong. ripgrep's regex engine uses both DFAs and NFAs, where DFAs are the work horse.

---

_Closed by @BurntSushi on 2018-02-16 13:40_

---
