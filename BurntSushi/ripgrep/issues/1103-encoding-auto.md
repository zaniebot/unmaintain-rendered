```yaml
number: 1103
title: "--encoding auto"
type: issue
state: closed
author: roblourens
labels:
  - doc
assignees: []
created_at: 2018-11-07T18:23:46Z
updated_at: 2019-01-26T19:13:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1103
synced_at: 2026-01-12T16:13:22Z
```

# --encoding auto

---

_@roblourens_

It's not really clear how the automatic encoding detection is supposed to work - which encodings should it be able to detect? Do you have any test cases that I can look at, or can you point to where the code is? It doesn't appear that encoding_rs is responsible for this, as best I can tell?

If I have a better idea of how it should work, I can file a better issue (or not file one)

---

_Label `doc` added by @BurntSushi on 2018-11-07 18:25_

---

_Comment by @BurntSushi on 2018-11-07 18:28_

Yeah, the man page is pretty light on details here, but I think the guide explains it a bit better: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#file-encoding Although, the guide never mentions the `auto` value, instead, it just talks about what ripgrep does by "default."

In summary, by default, ripgrep looks for a BOM. If it sees a UTF-16 BOM, then it does UTF-16 to UTF-8 transcoding automatically and searches the UTF-8. I don't think anything else is done.

Folks have filed issues in the past about being more aggressive in encoding detection, but I'd rather not dive into those waters if possible. It is possible for one to use `--pre` and probably `--pre-glob` to implement one's own detection & transcoding though.

---

_Comment by @roblourens on 2018-11-07 18:35_

Got it, yeah I think the man page implies that it might do more than it currently does.

---

_Closed by @BurntSushi on 2019-01-26 19:13_

---
