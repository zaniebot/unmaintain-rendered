```yaml
number: 718
title: Improve smartness of --smart-case
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/smart-case
created_at: 2017-12-18T21:13:11Z
updated_at: 2017-12-18T22:58:28Z
url: https://github.com/BurntSushi/ripgrep/pull/718
synced_at: 2026-01-12T18:23:13Z
```

# Improve smartness of --smart-case

---

_@okdana_

Fixes #717 (partially)

As suggested, i changed the smart-case feature in the `grep` crate to look for upper-case characters in the pattern itself rather than the AST. I also added some tests for it, and a note in the manual.

I didn't update the `grep` version number — you don't want me to do that, right?

---

_@BurntSushi reviewed on 2017-12-18 21:32_

---

_Review comment by @BurntSushi on `grep/src/search.rs`:323 on 2017-12-18 21:32_

This should accept a `&str`. I believe the callers won't need to be changed because of auto deref.

(Basically, you almost never want a `&String` or a `&Vec<T>`, and instead should prefer `&str` or `&[T]`. It obviously doesn't _really_ matter because this is an internal helper function, but I do want to stay idiomatic.)

---

_@BurntSushi reviewed on 2017-12-18 21:33_

---

_Review comment by @BurntSushi on `grep/src/search.rs`:332 on 2017-12-18 21:33_

This should just be `false` (no semi-colon) instead of `return false;`. :-)

---

_Review comment by @BurntSushi on `grep/src/search.rs`:394 on 2017-12-18 21:33_

Awesome, thank you for these tests!

---

_@BurntSushi requested changes on 2017-12-18 21:34_

This is great thanks! I have just a couple of very minor Rust-specific nits.

And yeah, don't update the version number. I do that as part of the release process.

---

_Comment by @okdana on 2017-12-18 21:36_

Oops. Fixed i think

---

_@BurntSushi approved on 2017-12-18 21:41_

Looks great! Thanks for the fixes!

---

_Merged by @BurntSushi on 2017-12-18 22:58_

---

_Closed by @BurntSushi on 2017-12-18 22:58_

---
