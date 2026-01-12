```yaml
number: 1788
title: Ripgrep slow on single core CPU
type: issue
state: closed
author: madnight
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2021-01-25T09:58:17Z
updated_at: 2021-02-23T21:57:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1788
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep slow on single core CPU

---

_@madnight_

Ripgrep is very slow on a single core cpu if it has to search a few gigabytes of text. I think this because of the low thread size default, in case ripgrep detects only one core.

However, since it is almost always the hard drive that is the slow part, increasing the thread count e.g. to -j 16 provides an incredible boost of about x6-8 times faster rg search on my machine.

You should be able to reproduce the results with by spinning up a qemu VM with a single core or test it on a very low-end VPS.

---

_Comment by @BurntSushi on 2021-01-25 12:55_

I think this is probably why the `-j` flag exists. My guess is that most ripgrep searches are searching data in the OS's page cache and not reading the bulk of it from disk. My other guess is that using ripgrep on single core CPUs is not terribly common.

Changing ripgrep's settings here requires a detailed investigation. I'm not sure when I'll have time to do that.

---

_Label `enhancement` added by @BurntSushi on 2021-01-25 12:55_

---

_Label `help wanted` added by @BurntSushi on 2021-01-25 12:55_

---

_Comment by @madnight on 2021-01-25 13:01_

> I think this is probably why the -j flag exists. 

Sure, but if there is a default that is up to 8x times faster for an not so uncommon use case, then I would probably pick that.

> My other guess is that using ripgrep on single core CPUs is not terribly common.

You might underestimate how many users like me have a cheap 1 CPU VPS in the cloud for a few bucks/month.

> Changing ripgrep's settings here requires a detailed investigation. I'm not sure when I'll have time to do that.

Sure, I understand that.

---

_Comment by @BurntSushi on 2021-02-23 21:57_

I'm going to close this since this setting is not something I'm planning on changing, and I don't really intend to do a thorough investigation of this myself. The OP talks about the hard disk being the slow aspect of the search, and while true, ripgrep tends to optimize for the case where its corpus is in memory. That is, many of ripgrep's optimizations don't have much impact if I/O is slow.

If someone else wants to do a thorough investigation here and make a solid case for changing the current defaults, then I'd be open to that. But a simple request isn't enough.

---

_Closed by @BurntSushi on 2021-02-23 21:57_

---
