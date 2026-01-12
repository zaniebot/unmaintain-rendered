```yaml
number: 2531
title: Debian installation documentation is a little outdated
type: issue
state: closed
author: briskt
labels:
  - doc
assignees: []
created_at: 2023-06-12T08:09:53Z
updated_at: 2023-06-12T11:53:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2531
synced_at: 2026-01-12T16:13:24Z
```

# Debian installation documentation is a little outdated

---

_@briskt_

#### Describe your feature request

Being my first day learning about ripgrep, I am very excited to know that it exists. Thank you for creating and maintaining it!!!

I just about installed it using the .deb download, but thankfully I read on and found that it is officially maintained by Debian. That, of course, means that it is available in Debian/Ubuntu derivatives like PopOS. Since Buster is now double-superseded, the whole Debian section of the install docs could probably be replaced by a simple `sudo apt install ripgrep` with the appropriate note about derivative distributions. Just my two cents.

Thanks again!

---

_Renamed from "Documentation request" to "Install documentation request" by @briskt on 2023-06-12 08:10_

---

_Closed by @BurntSushi on 2023-06-12 11:51_

---

_Comment by @BurntSushi on 2023-06-12 11:52_

Thanks. I made a small tweak to the wording, but the rest of it really should stay. The `deb` package installation steps, for example, are important because Debian usually has very old versions of software. The `deb` package provides an easy way for users to opt into something newer.

---

_Renamed from "Install documentation request" to "Debian installation documentation is a little outdated" by @BurntSushi on 2023-06-12 11:52_

---

_Label `doc` added by @BurntSushi on 2023-06-12 11:53_

---
