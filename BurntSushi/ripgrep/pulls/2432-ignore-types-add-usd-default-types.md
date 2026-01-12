```yaml
number: 2432
title: "ignore/types: add USD default types"
type: pull_request
state: closed
author: marksisson
labels:
  - rollup
assignees: []
base: master
head: marksisson/add-usd-file-types
created_at: 2023-02-27T14:09:44Z
updated_at: 2023-07-08T22:52:54Z
url: https://github.com/BurntSushi/ripgrep/pull/2432
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add USD default types

---

_@marksisson_

[USD](https://www.pixar.com/usd) is open-source software that can robustly and scalably interchange 3D scenes that may be composed of many different assets, sources, and animations, while fostering highly collaborative workflows.

---

_Comment by @BurntSushi on 2023-07-07 15:59_

This is not particularly clear to me.

Firstly, the better link seems to be this: https://openusd.org/release/index.html

Second, ripgrep generally only works with plain text formats, and it's not at all clear that USD is a plain text format. I had to hunt around to find an actual example: https://github.com/PixarAnimationStudios/OpenUSD/blob/release/pxr/usd/usd/testenv/testUsdBug141491.testenv/root.usda

So I guess this LGTM.

---

_Label `rollup` added by @BurntSushi on 2023-07-07 16:00_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
