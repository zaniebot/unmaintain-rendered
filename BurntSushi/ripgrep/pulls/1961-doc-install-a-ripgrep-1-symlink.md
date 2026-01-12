```yaml
number: 1961
title: "doc: install a ripgrep.1 symlink"
type: pull_request
state: closed
author: strugee
labels: []
assignees: []
base: master
head: ripgrep.1-symlink
created_at: 2021-08-03T04:47:43Z
updated_at: 2021-08-04T11:59:39Z
url: https://github.com/BurntSushi/ripgrep/pull/1961
synced_at: 2026-01-12T18:23:14Z
```

# doc: install a ripgrep.1 symlink

---

_@strugee_

This makes it easier to find the manpage when you first install ripgrep, since `man ripgrep` will work.

This is basically the first Rust code I've ever written; please don't assume I know what I'm doing (but this is so trivial it seems difficult to screw up, hopefully?). I did, however, test both of the modified functions.

---

_Comment by @BurntSushi on 2021-08-03 18:31_

I don't think it's wise to do this. The binary name is `rg` and that's therefore what the man page lookup symbol should be too. I don't believe it's common practice to create aliases like this. And I could easily imagine the existence of an alias like this leading folks to believe that the binary name of the tool is `ripgrep`. Finally, from an implementation perspective, this doesn't create a symlink on Windows (creating symlinks on Windows is a huge PIA).

So I think I'm going to pass on this. Thanks for the thought though!

---

_Closed by @BurntSushi on 2021-08-03 18:31_

---

_Comment by @strugee on 2021-08-03 20:23_

Sure, makes sense. (FWIW symlinks being a PITA is why I just didn't bother with implementing that.)

In that case though, can we find a way to make `apropos ripgrep` work? I think even if `apropos grep` brought up `rg(1)` that would be a huge improvement. I can file a follow-up ticket if you're open to a change like that.

---

_Branch deleted on 2021-08-03 20:23_

---

_Comment by @BurntSushi on 2021-08-04 11:59_

Getting `apropos ripgrep` to work seems reasonable. Filing a ticket for that sounds good. I don't know what work goes into it and probably won't have time to do it myself, so I can tag it as "help wanted."

I would also expect some light research into what is culturally acceptable. I wouldn't want `apropos grep` to show `rg(1)` as a hit unless it was common for such queries to produce alternatives to programs.

---
