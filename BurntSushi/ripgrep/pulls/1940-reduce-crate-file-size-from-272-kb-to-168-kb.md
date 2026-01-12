```yaml
number: 1940
title: "Reduce `.crate` file size from 272 KB to 168 KB"
type: pull_request
state: closed
author: Svetlitski
labels:
  - rollup
assignees: []
base: master
head: reduce-crate-size
created_at: 2021-07-19T23:44:02Z
updated_at: 2023-07-10T22:18:43Z
url: https://github.com/BurntSushi/ripgrep/pull/1940
synced_at: 2026-01-12T18:23:14Z
```

# Reduce `.crate` file size from 272 KB to 168 KB

---

_@Svetlitski_

The `ripgrep` crate currently includes test data, benchmark results, continuous integration configuration, and a few other miscellaneous items in its `.crate` file, bloating its size significantly ([crates.io](https://crates.io/crates/ripgrep) shows it presently weighs in at 272 KB). The changes in this PR ensure that these files are excluded from the `.crate` file going forward.

---

_Review comment by @BurntSushi on `Cargo.toml`:16 on 2021-07-20 00:16_

Could you make sure we don't exceed 79 columns here? e.g.,

```toml
exclude = [
    "foo",
    "bar",
    ...
]
```

Also, please drop `/tests/data` from this list. Dropping that means you can't run tests using just the crate download.

Ignoring the rest looks sensible though. Thanks!

---

_@BurntSushi requested changes on 2021-07-20 00:16_

---

_Comment by @Svetlitski on 2021-07-20 00:45_

Requested changes have been made. The `.crate` file is now 168 KB if you were curious.

---

_Review requested from @BurntSushi by @Svetlitski on 2021-07-20 00:45_

---

_Renamed from "Reduce `.crate` file size from 272 KB to 154 KB" to "Reduce `.crate` file size from 272 KB to 168 KB" by @Svetlitski on 2021-07-20 00:46_

---

_Comment by @Svetlitski on 2022-03-18 23:29_

@BurntSushi Bump on this. I believe I made all the requested changes. Would appreciate a review when you find the time.

---

_@BurntSushi approved on 2023-07-08 18:28_

Thanks!

---

_Label `rollup` added by @BurntSushi on 2023-07-08 18:29_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---

_Branch deleted on 2023-07-10 22:18_

---
