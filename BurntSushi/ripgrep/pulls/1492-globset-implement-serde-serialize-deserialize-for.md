```yaml
number: 1492
title: "globset: Implement serde::{Serialize, Deserialize} for Glob"
type: pull_request
state: merged
author: LPGhatguy
labels: []
assignees: []
merged: true
base: master
head: glob-serde
created_at: 2020-02-19T01:55:15Z
updated_at: 2020-02-21T12:40:48Z
url: https://github.com/BurntSushi/ripgrep/pull/1492
synced_at: 2026-01-12T18:23:13Z
```

# globset: Implement serde::{Serialize, Deserialize} for Glob

---

_@LPGhatguy_

Closes #1455.

This PR adds an optional dependency to serde, which implies a matching feature named `serde`. When enabled, `Serialize` and `Deserialize` are implemented for `Glob`, using its string form.

Implementing serde traits for `GlobSet` might also be a good idea, but looked like it might be a much more invasive change.

I used serde_json for the included test. There might be other edge cases that should be tested.

---

_Review comment by @BurntSushi on `Cargo.lock`:443 on 2020-02-20 21:09_

Is this version bump necessary? If not, please don't do it. I try to make sure to update all dependencies locally so that I can audit changes carefully.

---

_Review comment by @BurntSushi on `crates/globset/Cargo.toml`:27 on 2020-02-20 21:09_

Could you please create a `serde1` feature instead of tying the feature name to the name of a crate? This is useful if e.g., serde support for some reason requires an additional dependency.

---

_@BurntSushi requested changes on 2020-02-20 21:11_

Thanks! I think this looks okay. Could you add a section to globset's README that documents this feature?

Also, could you rebase this on master so that this PR gets the new CI infrastructure?

Thanks!

---

_@LPGhatguy reviewed on 2020-02-21 00:39_

---

_Review comment by @LPGhatguy on `Cargo.lock`:443 on 2020-02-21 00:39_

Sorry, I added the latest `serde_json` because it was a new dev dependency for this crate, I can reduce its minimum version req and undo the lockfile changes.

---

_Comment by @LPGhatguy on 2020-02-21 00:58_

Thanks for taking a look at my change!

I believe I've made the requested changes.

---

_@BurntSushi approved on 2020-02-21 12:40_

All right, LGTM! Thanks very much!

---

_Merged by @BurntSushi on 2020-02-21 12:40_

---

_Closed by @BurntSushi on 2020-02-21 12:40_

---
