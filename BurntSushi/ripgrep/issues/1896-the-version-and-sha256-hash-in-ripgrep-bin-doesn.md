```yaml
number: 1896
title: "The version and SHA256 hash in ripgrep-bin doesn't match."
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2021-06-15T14:18:16Z
updated_at: 2021-07-03T19:41:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1896
synced_at: 2026-01-12T16:13:24Z
```

# The version and SHA256 hash in ripgrep-bin doesn't match.

---

_@ghost_

The version is still 12.1.1 instead of 13.0.0:
https://github.com/BurntSushi/ripgrep/blob/7ce66f73cf7e76e9f2557922ac8e650eb02cf4ed/pkg/brew/ripgrep-bin.rb#L2

---

_Closed by @BurntSushi on 2021-06-15 14:30_

---

_Comment by @BurntSushi on 2021-06-15 14:34_

@arzoriac Out of curiosity, is there a reason why you're still using the tap instead of upstream homebrew? (I have idly considered removing the tap.)

---

_Comment by @ghost on 2021-06-15 14:56_

Just because I remember it having SIMD optimizations so I always had that it was a "better" version of it.

---

_Comment by @BurntSushi on 2021-06-15 14:58_

@arzoriac Ah. That was true a few years ago. You can switch to the Homebrew now then if that's the only reason. ripgrep's SIMD is now baked into the binary portably. Previously, you had to enable a compile time switch to get it to work on nightly Rust, but that stuff stabilized a while back and now everything "just works." :-)

---

_Comment by @ghost on 2021-06-15 16:16_

Good to know! I'll use it from Homebrew from now on.

---
