```yaml
number: 307
title: Minor code and doc fixes
type: pull_request
state: merged
author: mernen
labels: []
assignees: []
merged: true
base: master
head: minor-fixes
created_at: 2017-01-08T21:19:48Z
updated_at: 2017-01-08T22:02:58Z
url: https://github.com/BurntSushi/ripgrep/pull/307
synced_at: 2026-01-12T18:23:12Z
```

# Minor code and doc fixes

---

_@mernen_

The first commit is an oversight I spotted immediately after having #239 accepted.

The second one is just a minor divergence between the documentation and the actual code ([`Args::threads()`](/BurntSushi/ripgrep/blob/851799f42be4409d4e2498960725e11d0514a0e7/src/args.rs#L667)). I'm assuming the code has the actual intended cap, as it's not too uncommon to find desktop machines with 12+ logical cores, it's still low enough not to accidentally overwhelm a modest (albeit modern) CPU, and it seems to be the more recent commit.

---

_Renamed from "Minor fixes" to "Minor code and doc fixes" by @mernen on 2017-01-08 21:20_

---

_Comment by @BurntSushi on 2017-01-08 21:40_

@mernen Thanks! :-)

> I'm assuming the code has the actual intended cap, as it's not too uncommon to find desktop machines with 12+ logical cores, it's still low enough not to accidentally overwhelm a modest (albeit modern) CPU, and it seems to be the more recent commit.

Thanks for noticing this. The docs did indeed get out of sync. Note though that the default isn't necessarily 12, it's just [*capped* at 12](https://github.com/BurntSushi/ripgrep/blob/master/src/args.rs#L667).

---

_Merged by @BurntSushi on 2017-01-08 22:02_

---

_Closed by @BurntSushi on 2017-01-08 22:02_

---
