```yaml
number: 3038
title: Windows aarch64
type: pull_request
state: closed
author: jcotton42
labels:
  - rollup
assignees: []
base: master
head: windows-aarch64
created_at: 2025-04-29T21:04:13Z
updated_at: 2025-09-20T01:08:34Z
url: https://github.com/BurntSushi/ripgrep/pull/3038
synced_at: 2026-01-12T18:23:15Z
```

# Windows aarch64

---

_@jcotton42_

Closes #2943.

I tested the CI and release in my own repo, you can see the runs for CI here https://github.com/jcotton42/ripgrep/actions/runs/14741001100, and release here https://github.com/jcotton42/ripgrep/actions/runs/14741113883.

The test release created can be viewed here https://github.com/jcotton42/ripgrep/releases/tag/untagged-e5299d30ab039b6ee818.

I do not have an aarch64 Windows machine to test with at the moment, but I can confirm the architecture of `rg.exe` is arm64 per Git Bash's `file`, and it appears the test suite succeeded in CI.

This PR only adds MSVC support for aarch64, not GNU. Looking at the [available toolchains](https://rust-lang.github.io/rustup-components-history/), it seems the only GNU toolchain for Windows aarch64 is the -gnullvm version, which ripgrep is not using at the moment.

---

_Comment by @vishvanatarajan on 2025-06-21 07:59_

hi, just wondering any specific reason for not merging this PR?

---

_Comment by @tmm1 on 2025-07-24 17:00_

Friendly ping, there are a lot of arm64 windows laptops on the market now.

---

_@BurntSushi approved on 2025-07-27 16:48_

---

_Label `rollup` added by @BurntSushi on 2025-07-27 16:48_

---

_Comment by @bolinfest on 2025-09-02 22:17_

@BurntSushi any chance of cutting a new release that would now include the Windows ARM binaries?

---

_Comment by @BurntSushi on 2025-09-02 22:39_

https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#when-is-the-next-release

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
