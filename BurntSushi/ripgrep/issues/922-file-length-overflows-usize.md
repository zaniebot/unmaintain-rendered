```yaml
number: 922
title: "\"file length overflows usize\""
type: issue
state: closed
author: K-arti-k
labels:
  - question
  - libripgrep
assignees: []
created_at: 2018-05-19T19:35:42Z
updated_at: 2018-08-20T11:10:22Z
url: https://github.com/BurntSushi/ripgrep/issues/922
synced_at: 2026-01-12T16:13:22Z
```

# "file length overflows usize"

---

_@K-arti-k_

**#### What version of ripgrep are you using?**

`ripgrep 0.7.1
-AVX -SIMD`.

**#### What operating system are you using ripgrep on?**

Operating System: 64-bit
Windows 7: Cygwin Terminal

**#### Describe your question, feature request, or bug.**

With specific files when trying to grep I get an error saying "file length overflows usize"

**#### If this is a bug, what are the steps to reproduce the behavior?**

$ ./rg -i -w "Gurkiran" "zoosk.txt"
file length overflows usize

---

_Comment by @BurntSushi on 2018-05-19 21:03_

Sorry, but this isn't reproducible. Please provide enough information (like the file you're searching) in order to reproduce the problem. The issue template you filled out explicitly requests this:

> If possible, please include both your search patterns and the corpus on which
you are searching. Unless the bug is very obvious, then it is unlikely that it
will be fixed if the ripgrep maintainers cannot reproduce it.
>
> If the corpus is too big and you cannot decrease its size, file the bug anyway
and the ripgrep maintainers will help figure out next steps.

Please also state how you installed ripgrep. Please also try the latest version, which is 0.8.1.

---

_Label `question` added by @BurntSushi on 2018-05-19 21:03_

---

_Comment by @peter-bertok on 2018-07-05 06:20_

This is indirectly caused by this issue in memmap-rs: https://github.com/danburkert/memmap-rs/issues/67

Someone already submitted a patch for memmap-rs back in April 2018, but fixing this for the general case will take some time...

---

_Comment by @danburkert on 2018-07-05 16:46_

Something doesn't add up here.  Ripgrep isn't using file offsets, so the usize/u64 offset API issue in memmap shouldn't be in play here.  That error message only occurs when you attempt to memory map a file whose length exceeds `usize::MAX`.  That shouldn't be possible on a 64-bit OS.

---

_Comment by @BurntSushi on 2018-07-05 16:58_

@danburkert Yeah, something is definitely fishy with thia bug report. Either the info is probably wrong, or Windows is running in 32 bit mode or maybe even they downloaded the 32 bit executable? I don't know.

---

_Comment by @BatmanAoD on 2018-07-05 21:37_

Per @peter-bertok's [comment in users.rust-lang](https://users.rust-lang.org/t/rust-beginner-notes-questions/14928/78?u=batmanaod), this is indeed reproducible with the 32 bit executable, so I expect @K-arti-k did indeed encounter this issue with the 32 bit executable.

I feel that searching through a multi-Gb file in 32-bit mode (especially on a 64-bit OS!) is not a particularly compelling use-case for RipGrep. That said, Peter Bertok is presumably correct that this could eventually be fixed in a future version of `memmap-rs`.

---

_Comment by @BurntSushi on 2018-07-05 21:54_

@BatmanAoD Note that this bug should be fixed on master. ripgrep just won't use memory maps and use standard read calls instead. (Also, I'm not sure that a fix to this belongs in the `memmap` crate.)

---

_Comment by @danburkert on 2018-07-05 22:07_

> That said, Peter Bertok is presumably correct that this could eventually be fixed in a future version of memmap-rs.

No, this is not correct.  This ripgrep issue has no relation to the u64/usize offset API issue, which is the only `memmap` bug which has been brought forward.  It's fundamentally not possible to create a memory map with a length which exceeds `usize::MAX`.

---

_Comment by @BatmanAoD on 2018-07-05 22:18_

@danburkert In the linked discussion (which unfortunately is long and contentious, and has a broad range of topics that are mostly unrelated), [@peter-bertok claims](https://users.rust-lang.org/t/rust-beginner-notes-questions/14928/83?u=batmanaod) that 32-bit systems *can* create memory maps for *arbitrarily* sized files, using "views" that fit into the actual available memory space (which is really `isize::MAX` rather than `usize::MAX` for some or possibly all 32-bit Windows OSes).

---

_Label `libripgrep` added by @BurntSushi on 2018-07-22 14:47_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
