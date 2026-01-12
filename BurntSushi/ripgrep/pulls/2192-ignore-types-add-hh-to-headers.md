```yaml
number: 2192
title: "ignore/types: Add \"*.hh\" to headers"
type: pull_request
state: merged
author: cemeyer
labels: []
assignees: []
merged: true
base: master
head: add_hh_headers
created_at: 2022-04-23T16:35:49Z
updated_at: 2022-04-25T14:40:02Z
url: https://github.com/BurntSushi/ripgrep/pull/2192
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: Add "*.hh" to headers

---

_@cemeyer_

Like .hpp, .hh is an occasionally used extension for C++ headers
(to distinguish them from C headers).

---

_Comment by @BurntSushi on 2022-04-24 17:20_

Can you show me something that uses `.hh` for this purpose?

---

_Comment by @cemeyer on 2022-04-24 18:07_

Sure.  There are a handful (<10) of these in FreeBSD's src tree -- e.g., https://github.com/freebsd/freebsd-src/blob/main/sbin/devd/devd.hh .  Others:

```
$ find . -name \*.hh
./sbin/devd/devd.hh
./tests/sys/fs/fusefs/utils.hh
./tests/sys/fs/fusefs/mockfs.hh
./usr.bin/dtc/checking.hh
./usr.bin/dtc/dtb.hh
./usr.bin/dtc/fdt.hh
./usr.bin/dtc/input_buffer.hh
./usr.bin/dtc/util.hh
```

Some reference to the naming convention here: https://docs.fileformat.com/programming/hh/

---

_Merged by @BurntSushi on 2022-04-25 11:38_

---

_Closed by @BurntSushi on 2022-04-25 11:38_

---

_Comment by @BurntSushi on 2022-04-25 11:38_

Lovely, thank you! TIL. :-)

---

_Comment by @cemeyer on 2022-04-25 14:38_

Thanks! They were new to me, too — I was trying to locate a macro definition and `rg -t h` was telling me it didn’t exist 😀.

---

_Branch deleted on 2022-04-25 14:40_

---
