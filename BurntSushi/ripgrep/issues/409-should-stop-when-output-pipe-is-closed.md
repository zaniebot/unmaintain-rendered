```yaml
number: 409
title: Should stop when output pipe is closed
type: issue
state: closed
author: glandium
labels:
  - duplicate
assignees: []
created_at: 2017-03-15T05:12:08Z
updated_at: 2017-03-15T10:53:27Z
url: https://github.com/BurntSushi/ripgrep/issues/409
synced_at: 2026-01-12T16:13:21Z
```

# Should stop when output pipe is closed

---

_@glandium_

I was doing something like:
```
$ ssh host cat verylargearchive.zst | zstd -d | rg pattern | head
```
to check that my pattern made sense before running on the full thing. And that hang for a while, and I assume it would have waited for the whole thing to have been read.

Replace `rg` with `grep` and the program stops as soon as `head` closes its input pipe.

---

_Comment by @BurntSushi on 2017-03-15 10:53_

Yes, this is a known bug. Dupe of #200.

---

_Closed by @BurntSushi on 2017-03-15 10:53_

---

_Label `duplicate` added by @BurntSushi on 2017-03-15 10:53_

---
