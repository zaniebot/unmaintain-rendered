```yaml
number: 805
title: "Implement underline support in termcolor (closes #798)"
type: pull_request
state: merged
author: balajisivaraman
labels: []
assignees: []
merged: true
base: master
head: fix_798
created_at: 2018-02-14T16:56:51Z
updated_at: 2018-02-20T15:43:16Z
url: https://github.com/BurntSushi/ripgrep/pull/805
synced_at: 2026-01-12T18:23:13Z
```

# Implement underline support in termcolor (closes #798)

---

_@balajisivaraman_

Closes #798 

This PR implements underline support in `termcolor`. Hopefully I've done it right because it seemed straightforward enough to implement. 😄

I tested this by doing `./target/release/rg 'bufwtr' --colors 'match:style:underline'` on my PC after building `rg`, and I was able to see underlined text.

---

_@BurntSushi approved on 2018-02-14 17:01_

This looks perfect! Great work!

---

_Merged by @BurntSushi on 2018-02-20 12:10_

---

_Closed by @BurntSushi on 2018-02-20 12:10_

---

_Branch deleted on 2018-02-20 15:43_

---
