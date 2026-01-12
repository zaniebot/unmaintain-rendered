```yaml
number: 688
title: fix a bunch of clippy warnings
type: pull_request
state: merged
author: matthiaskrgr
labels: []
assignees: []
merged: true
base: master
head: clippy
created_at: 2017-11-19T16:37:25Z
updated_at: 2017-11-22T16:26:51Z
url: https://github.com/BurntSushi/ripgrep/pull/688
synced_at: 2026-01-12T18:23:13Z
```

# fix a bunch of clippy warnings

---

_@matthiaskrgr_

_No description provided._

---

_Review comment by @BurntSushi on `src/main.rs`:140 on 2017-11-20 12:27_

Please drop this entire commit. I prefer the existing style.

---

_@BurntSushi requested changes on 2017-11-20 12:29_

Thanks! Most of these changes look OK to me, but I'd like to request that you drop commit `60bde7` from this PR, as I prefer the previous style.

---

_@matthiaskrgr reviewed on 2017-11-20 15:51_

---

_Review comment by @matthiaskrgr on `src/main.rs`:140 on 2017-11-20 15:51_

done :)

---

_Comment by @matthiaskrgr on 2017-11-21 22:39_

I added another commit that adds clippy as optional dependency.
```` cargo build --features clippy```` will compile with static analysis :)

---

_Review comment by @BurntSushi on `Cargo.toml`:36 on 2017-11-22 11:51_

Sorry, but please remove this and any corresponding `cfg`s. I don't want to add a very unstable linter to ripgrep's dependencies.

---

_@BurntSushi requested changes on 2017-11-22 11:52_

---

_@matthiaskrgr reviewed on 2017-11-22 12:37_

---

_Review comment by @matthiaskrgr on `Cargo.toml`:36 on 2017-11-22 12:37_

This is not a default dependency, clippy is *only* pulled in if we explicitly build with "--features clippy"

---

_Review comment by @BurntSushi on `Cargo.toml`:36 on 2017-11-22 12:39_

Yes, I understand. I still don't want it.

---

_@BurntSushi reviewed on 2017-11-22 12:39_

---

_@matthiaskrgr reviewed on 2017-11-22 12:41_

---

_Review comment by @matthiaskrgr on `Cargo.toml`:36 on 2017-11-22 12:41_

Alright, I removed it.

---

_@BurntSushi approved on 2017-11-22 12:42_

Great, thanks. :-)

---

_Merged by @BurntSushi on 2017-11-22 15:50_

---

_Closed by @BurntSushi on 2017-11-22 15:50_

---

_Branch deleted on 2017-11-22 16:26_

---
