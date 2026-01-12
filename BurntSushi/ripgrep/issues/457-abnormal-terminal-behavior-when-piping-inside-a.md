```yaml
number: 457
title: abnormal terminal behavior when piping inside a bash function
type: issue
state: closed
author: maverickwoo
labels: []
assignees: []
created_at: 2017-04-21T18:57:13Z
updated_at: 2017-04-23T03:29:01Z
url: https://github.com/BurntSushi/ripgrep/issues/457
synced_at: 2026-01-12T16:13:22Z
```

# abnormal terminal behavior when piping inside a bash function

---

_@maverickwoo_

My goal is to define a bash function `rg` that will run `less` automatically. However, it seems to cause strange terminal behavior described below. I am able to reproduce this in the macOS environment provided at the end of the post.

This is with a fresh install of ripgrep 0.5.1 as of this writing with rustc 1.6.0 toolchain. The following works as expected. I have unset all less related environment variables `LESS` and `LESSOPEN`.
```bash
$ function rg () { command rg -Hn "$@" | less; }
$ echo abc | command rg a | less
abc
```
But the following results in unexpected terminal behavior:
```bash
$ echo abc | rg a

[1]+  Stopped                 echo abc | rg a
```

I am not sure how to debug this further and would appreciate any hint. Here is the configuration I used for repro:
```
Darwin [foo] 13.4.0 Darwin Kernel Version 13.4.0: Mon Jan 11 18:17:34 PST 2016; root:xnu-2422.115.15~1/RELEASE_X86_64 x86_64
GNU bash, version 4.4.12(1)-release (x86_64-apple-darwin13.4.0)
less 487 (PCRE regular expressions)
```

Thanks!

---

_Comment by @maverickwoo on 2017-04-23 03:29_

The bug is actually in iTerm2 shell integration. I will file the bug there when I figure it out.

Sorry!

---

_Closed by @maverickwoo on 2017-04-23 03:29_

---
