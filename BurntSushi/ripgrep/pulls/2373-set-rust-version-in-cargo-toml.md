```yaml
number: 2373
title: Set rust-version in Cargo.toml
type: pull_request
state: merged
author: atouchet
labels: []
assignees: []
merged: true
base: master
head: ver
created_at: 2022-12-20T23:08:53Z
updated_at: 2022-12-21T19:19:23Z
url: https://github.com/BurntSushi/ripgrep/pull/2373
synced_at: 2026-01-12T18:23:14Z
```

# Set rust-version in Cargo.toml

---

_@atouchet_

This MSRV was bumped higher than 1.56 in 8905d54a9f25f4c1e4e3ca8331f517473e174d87. Should this be added to any other crates as well?

---

_@BurntSushi approved on 2022-12-21 12:36_

> Should this be added to any other crates as well?

Probably, for crates with an MSRV at or greater than 1.56. I'm not sure if many of mine have reached that point.

---

_Merged by @BurntSushi on 2022-12-21 12:37_

---

_Closed by @BurntSushi on 2022-12-21 12:37_

---

_Branch deleted on 2022-12-21 19:19_

---
