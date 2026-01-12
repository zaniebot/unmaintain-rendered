```yaml
number: 2600
title: "ci: replace mips with powerpc64, aarch64 and s390x"
type: pull_request
state: closed
author: BurntSushi
labels: []
assignees: []
base: master
head: ag/fix-ci
created_at: 2023-08-29T00:19:48Z
updated_at: 2023-08-29T02:13:42Z
url: https://github.com/BurntSushi/ripgrep/pull/2600
synced_at: 2026-01-12T18:23:14Z
```

# ci: replace mips with powerpc64, aarch64 and s390x

---

_@BurntSushi_

We drop our MIPS target because it no longer works.[1] We were previously using it as a means of testing ripgrep in a big endian environment. So to achieve that without MIPS, we test on powerpc64 and s390x. (No particular reason to do both, but why not.)

We also add aarch64 as a proxy for at least ensuring everything works for the same architecture as Apple silicon. It's not a guarantee that everything works, but it seems better than nothing until we can actually test Apple silicon in CI.

[1]: https://github.com/rust-lang/regex/commit/c788378d6fe407f4774df98a78436cea5d98525b

---

_Closed by @BurntSushi on 2023-08-29 02:13_

---
