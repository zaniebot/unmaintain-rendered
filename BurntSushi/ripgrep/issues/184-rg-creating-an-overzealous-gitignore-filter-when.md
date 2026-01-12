```yaml
number: 184
title: rg creating an overzealous .gitignore filter when used in a sub-directory
type: issue
state: closed
author: tarka
labels:
  - bug
assignees: []
created_at: 2016-10-16T11:55:32Z
updated_at: 2016-10-16T14:15:32Z
url: https://github.com/BurntSushi/ripgrep/issues/184
synced_at: 2026-01-12T18:23:11Z
```

# rg creating an overzealous .gitignore filter when used in a sub-directory

---

_@tarka_

To reproduce:

```
$ cd linux
$ rg TURKS
<SNIP>

drivers/gpu/drm/radeon/radeon_uvd.c
111:    case CHIP_TURKS:

<SNIP>

$ cd drivers/gpu/drm/radeon/
$ rg TURKS
$
```

The relevant debug output is:

```
DEBUG:rg::ignore: linux-drm-intel/drivers/gpu/drm/./radeon_uvd.c ignored by Pattern { from: "/home/ssmith/software/linux-drm-intel/.gitignore", original: ".*", pat: "**/.*", whitelist: false, only_dir: false }
```

Full debug log attached.
[rg.txt](https://github.com/BurntSushi/ripgrep/files/531817/rg.txt)


---

_Comment by @BurntSushi on 2016-10-16 12:57_

What version of rg are you using? The latest is 0.2.3.

On Oct 16, 2016 7:55 AM, "Steve Smith" notifications@github.com wrote:

> To reproduce:
> 
> $ cd linux
> $ rg TURKS
> <SNIP>
> 
> drivers/gpu/drm/radeon/radeon_uvd.c
> 111:    case CHIP_TURKS:
> 
> <SNIP>
> 
> $ cd drivers/gpu/drm/radeon/
> $ rg TURKS
> $
> 
> The relevant debug output is:
> 
> DEBUG:rg::ignore: linux-drm-intel/drivers/gpu/drm/./radeon_uvd.c ignored by Pattern { from: "/home/ssmith/software/linux-drm-intel/.gitignore", original: "._", pat: "__/._", whitelist: false, only_dir: false }
> 
> Full debug log attached.
> rg.txt https://github.com/BurntSushi/ripgrep/files/531817/rg.txt
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/184, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34oUp4H6X71InKjHH042dujYGN2Lrks5q0hC0gaJpZM4KX9Bm
> .


---

_Comment by @tarka on 2016-10-16 13:04_

0.2.3 from cargo install, and also tested against master.


---

_Label `bug` added by @BurntSushi on 2016-10-16 13:48_

---

_Closed by @BurntSushi on 2016-10-16 14:15_

---

_Comment by @BurntSushi on 2016-10-16 14:15_

Good find. This was indeed a bug. Should be fixed in the next release!


---
