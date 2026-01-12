```yaml
number: 512
title: add yocto file types
type: pull_request
state: merged
author: sidalit
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-06-12T17:14:52Z
updated_at: 2017-06-19T22:08:54Z
url: https://github.com/BurntSushi/ripgrep/pull/512
synced_at: 2026-01-12T18:23:13Z
```

# add yocto file types

---

_@sidalit_

add file types for embedded linux Yocto project : https://www.yoctoproject.org/

---

_Merged by @BurntSushi on 2017-06-12 23:02_

---

_Closed by @BurntSushi on 2017-06-12 23:02_

---

_Comment by @BurntSushi on 2017-06-12 23:02_

Thanks so much!

---

_Comment by @lilianmoraru on 2017-06-19 18:13_

I would like to point 2 issues:
- Yocto is too generic. These files are specific to 1 tool, called BitBake.
- This is not a complete list. It also has files akin' to headers in C/C++, where the content is inserted directly inside the *.bb* files.

---

_Comment by @sidalit on 2017-06-19 18:47_

@lilianmoraru, thank you for pointing out these remarks, what do you suggest ?
Actually, I was thinking using `-tbb` but it is too explicit and may create confusion since people may think it only includes `.bb` files.

---

_Comment by @lilianmoraru on 2017-06-19 19:19_

This would be in the same ballpark as `sh`, `cpp`, `c`, `asm`, etc...

---

_Comment by @lilianmoraru on 2017-06-19 20:55_

I scrolled through the reference manual and it seems to have these extensions:
```
.bb
.bbappend
.bbclass
.conf
.inc
```
It also can call into Python code, but I guess this could be excluded.

---

_Comment by @sidalit on 2017-06-19 21:52_

Multiple languages and configurations use `.conf` (e.g `rc.conf`, `syslog.conf` etc...) and `.inc` (e.g in C/C++, PHP and VIM config) as file type. IMHO, they cannot be included in specific yocto/bitbake file types. @BurntSushi, what do you think ?

Edit: modifying the file type name from `yocto` to `bitbake` is ok for me if it can be more explicit.

---

_Comment by @BurntSushi on 2017-06-19 22:08_

I don't have a lot of domain knowledge here, but it is OK for file types to overlap. For example, `-tc` and `-tcpp` will both search `*.h` files.

---
