```yaml
number: 148
title: Change Arch Linux instructions
type: pull_request
state: merged
author: munyari
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2016-10-05T12:26:31Z
updated_at: 2019-09-11T05:07:06Z
url: https://github.com/BurntSushi/ripgrep/pull/148
synced_at: 2026-01-12T18:23:12Z
```

# Change Arch Linux instructions

---

_@munyari_

The `-Syu` flag will do a full system upgrade and then install the package, which is not necessarily the desired behavior. Only the `-S` flag is necessary to install a single package.
See https://wiki.archlinux.org/index.php/Pacman#Installing_specific_packages
https://wiki.archlinux.org/index.php/Pacman#Upgrading_packages


---

_Comment by @BurntSushi on 2016-10-05 12:31_

The `-Syu` was added by @svenstaro in #92. I also found it kind of odd. @svenstaro Thoughts?


---

_Comment by @svenstaro on 2016-10-05 13:01_

It's true that you only need -S but then again telling people to do -Syu is the safer bet because we do not support partial upgrades and we regularly get problems with that in IRC. You can take it out but someone will find a way to do a partial upgrade.


---

_Comment by @munyari on 2016-10-05 13:47_

Perhaps, but if that's considered best practice, shouldn't it be in the arch wiki?


---

_Comment by @svenstaro on 2016-10-05 13:50_

Perhaps so but I'm sure it says that somewhere. Anyway, I think @BurntSushi should accept this patch and then we'll just hope that no idiot turns up that manages to break something.


---

_Merged by @BurntSushi on 2016-10-05 13:53_

---

_Closed by @BurntSushi on 2016-10-05 13:53_

---

_Comment by @BurntSushi on 2016-10-05 13:53_

All right, let's do it. Thanks for working it out!


---

_Comment by @munyari on 2016-10-05 14:32_

@svenstaro no need to refer to anybody as an idiot mate. It's not the most obvious problem.


---
