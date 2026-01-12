```yaml
number: 1656
title: Use IO_uring where possible for increased performance?
type: issue
state: closed
author: boramalper
labels:
  - question
assignees: []
created_at: 2020-08-15T19:16:18Z
updated_at: 2020-08-15T19:27:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1656
synced_at: 2026-01-12T16:13:24Z
```

# Use IO_uring where possible for increased performance?

---

_@boramalper_

[Benchmarks show](https://www.phoronix.com/scan.php?page=news_item&px=Linux-5.6-IO-uring-Tests) that IO_uring can lead to a significant increase in raw I/O performance.

I am not experienced with ripgrep at all so it's hard for me to evaluate how CPU bound it is and how much a faster I/O subsystem can further increase its overall performance but it might be worth looking into.

---

_Comment by @BurntSushi on 2020-08-15 19:27_

No. Or at least, not without some compelling evidence that it would be worthwhile. In the common case, ripgrep is CPU bound, because you're usually using it to search things repeatedly (like a code repository). In the case where ripgrep isn't CPU bound, it's not clear why aync I/O would necessarily improve things. ripgrep already uses parallelism.

The benefits of io_uring in ripgrep's specific case would need to be _extremely significant_ to make it worthwhile to use. Not only would it be a boatload of platform specific code, but it would greatly complicate ripgrep's handling of I/O.

I have no plans to do this experiment myself, but others are welcome to do it.

---

_Closed by @BurntSushi on 2020-08-15 19:27_

---

_Label `question` added by @BurntSushi on 2020-08-15 19:27_

---
