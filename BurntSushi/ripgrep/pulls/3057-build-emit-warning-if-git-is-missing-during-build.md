```yaml
number: 3057
title: "build: emit warning if git is missing during build"
type: pull_request
state: closed
author: kevichi7
labels:
  - rollup
assignees: []
base: master
head: build-warning-if-no-git
created_at: 2025-05-29T12:41:29Z
updated_at: 2025-09-25T08:32:41Z
url: https://github.com/BurntSushi/ripgrep/pull/3057
synced_at: 2026-01-12T18:23:15Z
```

# build: emit warning if git is missing during build

---

_@kevichi7_

Adds a cargo:warning if git is not available during build.
This makes it easier to understand why RIPGREP_BUILD_GIT_HASH might be missing, without breaking the build.

---

_@BurntSushi approved on 2025-08-19 20:26_

Thanks! Note that using `eprintln!` here is incorrect, and causes this warning to get swallowed. You need to print the warning to `stdout`.

I also made a few other touch-ups in my rollup branch.

---

_Label `rollup` added by @BurntSushi on 2025-08-19 20:26_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

_Branch deleted on 2025-09-25 08:32_

---
