```yaml
number: 935
title: Bash script for updating Rust nightly and ripgrep
type: issue
state: closed
author: slaiyer
labels: []
assignees: []
created_at: 2018-06-02T19:38:52Z
updated_at: 2018-06-03T04:31:41Z
url: https://github.com/BurntSushi/ripgrep/issues/935
synced_at: 2026-01-12T16:13:22Z
```

# Bash script for updating Rust nightly and ripgrep

---

_@slaiyer_

I just wrote a Bash utility [rip_up](https://github.com/Slaiyer/rip_up):
- Calls rustup
- Pulls changes from [ripgrep](https://github.com/BurntSushi/ripgrep) upstream
- Rebuilds ripgrep (but only if required) with SIMD and AVX extensions
- Runs built-in tests
- Strips built executable

---

_Comment by @BurntSushi on 2018-06-02 20:13_

Sounds cool! What are you requesting here?

---

_Comment by @slaiyer on 2018-06-02 23:59_

Just leaving this here in case anyone comes looking (like I originally came looking for a PPA).
If you like it, please feel free to link to it in your README.

A token of my appreciation for a wonderful tool; thanks :)

---

_Comment by @BurntSushi on 2018-06-03 03:32_

Okay, thanks. Note that I distribute pre-compiled executables here on GitHub, so you don't have to install Rust.

---

_Closed by @BurntSushi on 2018-06-03 03:32_

---
