```yaml
number: 2330
title: Add install commands for ALT Linux
type: pull_request
state: closed
author: bircoph
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2022-10-10T18:31:28Z
updated_at: 2023-07-08T22:52:59Z
url: https://github.com/BurntSushi/ripgrep/pull/2330
synced_at: 2026-01-12T18:23:14Z
```

# Add install commands for ALT Linux

---

_@bircoph_

_No description provided._

---

_@BurntSushi reviewed on 2022-10-10 18:34_

---

_Review comment by @BurntSushi on `README.md`:302 on 2022-10-10 18:34_

Should this be `sudo apt-get` like the other `apt-get` instructions in this README?

---

_@bircoph reviewed on 2022-10-10 19:28_

---

_Review comment by @bircoph on `README.md`:302 on 2022-10-10 19:28_

I think no, because there are different ways of obtaining root in ALT: su, ssh, sudo. On many setups sudo is absent at all out of the box. I may change prompt from "$ " to "# " so it will be highlighted that root access is required. But I think users will understand what is necessary even with $.

---

_@BurntSushi reviewed on 2022-10-10 19:44_

---

_Review comment by @BurntSushi on `README.md`:302 on 2022-10-10 19:44_

All of that is true of other distros too. Please be consistent with other instructions and use sudo. Using `#` is waaaaay too subtle.

---

_Label `rollup` added by @BurntSushi on 2023-07-08 13:02_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
