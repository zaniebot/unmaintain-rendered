```yaml
number: 1023
title: "minor fixes & set version policy to \"latest stable release of Rust\""
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/various-fixes
created_at: 2018-08-22T02:16:02Z
updated_at: 2018-08-22T03:05:57Z
url: https://github.com/BurntSushi/ripgrep/pull/1023
synced_at: 2026-01-12T18:23:13Z
```

# minor fixes & set version policy to "latest stable release of Rust"

---

_@BurntSushi_

This PR provides a smattering of minor fixes and cleanups. The biggest change here is actually putting #1019 into action and changing ripgrep's minimum Rust version policy to the latest stable release.

Fixes #842 
Fixes #953 
Fixes #984 
Fixes #1019 
Fixes #1022 

ref #705 

---

_Review comment by @BurntSushi on `complete/_rg`:170 on 2018-08-22 02:32_

cc @okdana I made this change based on your docs. Let me know if I got this wrong!

---

_@BurntSushi reviewed on 2018-08-22 02:32_

---

_@okdana reviewed on 2018-08-22 02:41_

---

_Review comment by @okdana on `complete/_rg`:170 on 2018-08-22 02:41_

Oh, i was the one who got it wrong actually, sorry. :/

Neither of those exclusions are necessary since we've got the parentheses around the group name, but i *had* intended to exclude `--no-pcre2` (because there's no reason you'd change the PCRE2 Unicode mode if you were just going to disable PCRE2 anyway). I guess i just wasn't paying attention.

So both of the options in that group should actually have `(--no-pcre2)`.

---

_Merged by @BurntSushi on 2018-08-22 03:05_

---

_Closed by @BurntSushi on 2018-08-22 03:05_

---

_Branch deleted on 2018-08-22 03:05_

---
