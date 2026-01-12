```yaml
number: 2846
title: "add Makefile.inc to \"make\" type"
type: pull_request
state: closed
author: bad
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2024-06-28T22:22:36Z
updated_at: 2025-09-20T01:08:27Z
url: https://github.com/BurntSushi/ripgrep/pull/2846
synced_at: 2026-01-12T18:23:14Z
```

# add Makefile.inc to "make" type

---

_@bad_

The *BSD build systems make use of "Makefile.inc" a lot.  Make the "make" type recognize this file by default.

---

_Comment by @okdana on 2024-06-29 03:57_

You might consider generalising this to `Makefile.*` actually. It's common to use this naming convention for alternate Makefiles, e.g. for different targets. An extreme example of this is [RetroArch](https://github.com/libretro/RetroArch), but even [FreeBSD](https://github.com/freebsd/freebsd-src) has some other instances that the change in its current form wouldn't address

(If you did do it that way, i would suggest `Makefile.*` instead of `[Mm]akefile.*` because the latter seems too prone to false positives in this case)

---

_Comment by @bad on 2024-07-28 16:35_

Is there a reason this PR hasn't been merged yet?


---

_Comment by @BurntSushi on 2024-07-28 17:18_

Sometimes it just takes me a while.

Otherwise I think I agree with @okdana's feedback here.

---

_Comment by @bad on 2024-07-28 17:47_

Thanks for the quick response.

I'm fine with @okdana's suggestion and changed the code.


---

_Comment by @bad on 2025-01-14 16:25_

Is there anything I can do to get this merged?


---

_Label `rollup` added by @BurntSushi on 2025-07-26 13:52_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
