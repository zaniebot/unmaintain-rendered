```yaml
number: 2132
title: "Fix failing `nightly-arm` build on `ci` workflow"
type: pull_request
state: closed
author: arcsi42
labels: []
assignees: []
base: master
head: ci-fix-nightly-arm
created_at: 2022-01-19T23:26:19Z
updated_at: 2022-03-21T12:59:06Z
url: https://github.com/BurntSushi/ripgrep/pull/2132
synced_at: 2026-01-12T18:23:14Z
```

# Fix failing `nightly-arm` build on `ci` workflow

---

_@arcsi42_

Some tests in `nightly-arm` fail when `brotli` and `zstd` is not found.

Fixes #2130, but first a new docker image has to be built and pushed to `burntsushi/cross:arm-unknown-linux-gnueabihf`

---

_Comment by @arcsi42 on 2022-01-19 23:29_

Passing `ci` workflow can be found at https://github.com/arcsi42/ripgrep/actions/runs/1720707380.


---

_Renamed from "Ci fix `nightly-arm`" to "Fix failing `nightly-arm` build on `ci` workflow" by @arcsi42 on 2022-01-19 23:30_

---

_Closed by @BurntSushi on 2022-03-21 12:59_

---
