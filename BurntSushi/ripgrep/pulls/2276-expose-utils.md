```yaml
number: 2276
title: Expose utils
type: pull_request
state: closed
author: BatmanAoD
labels: []
assignees: []
draft: true
base: master
head: expose-utils
created_at: 2022-08-07T00:11:35Z
updated_at: 2023-07-08T13:11:57Z
url: https://github.com/BurntSushi/ripgrep/pull/2276
synced_at: 2026-01-12T18:23:14Z
```

# Expose utils

---

_@BatmanAoD_

Needed for https://github.com/BatmanAoD/RipPatch

---

_Comment by @BurntSushi on 2023-07-08 13:11_

I'm not comfortable exposing this as-is. It seems pretty out of place in the public API of this crate.

Looking at the code, it looks like you could probably copy/re-implement it without too much trouble.

---

_Closed by @BurntSushi on 2023-07-08 13:11_

---
