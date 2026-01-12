```yaml
number: 1239
title: "ignore/types: add lockfiles"
type: pull_request
state: merged
author: tpai
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2019-04-09T09:39:00Z
updated_at: 2019-04-10T01:33:47Z
url: https://github.com/BurntSushi/ripgrep/pull/1239
synced_at: 2026-01-12T18:23:13Z
```

# ignore/types: add lockfiles

---

_@tpai_

Add npm, yarn, composer and ruby gems lockfiles

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:199 on 2019-04-09 10:51_

Maybe call this `lock` instead? It's a bit shorter.

Also, you might as well add `Cargo.lock` to this.

And please make sure the style stays consistent with the surrounding code. i.e., No lines over 79 columns please.

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:164 on 2019-04-09 10:52_

I'm not so sure about this change. We also have `Cargo.lock` listed under the `toml` type, so this change is inconsistent with that. The thing is that we know `composer.lock` to be a JSON file, right? So it really should be searched when you ask to search for JSON files. Similarly for `Cargo.lock`, which we know to be TOML.

Maybe leave `composer.lock` here? We can still keep a separate overlapping `lock` type.

---

_@BurntSushi requested changes on 2019-04-09 10:52_

Thanks! I left a few comments.

---

_@tpai reviewed on 2019-04-09 12:33_

---

_Review comment by @tpai on `ignore/src/types.rs`:164 on 2019-04-09 12:33_

I see, I will leave `composer.lock` with no change.

---

_@BurntSushi approved on 2019-04-09 14:24_

LGTM, thanks!

---

_Merged by @BurntSushi on 2019-04-09 14:24_

---

_Closed by @BurntSushi on 2019-04-09 14:24_

---

_Branch deleted on 2019-04-10 01:33_

---
