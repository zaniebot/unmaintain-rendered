```yaml
number: 139
title: Make the repo a Homebrew Tap
type: pull_request
state: merged
author: moshen
labels: []
assignees: []
merged: true
base: master
head: make-a-tap
created_at: 2016-09-30T18:01:41Z
updated_at: 2016-10-03T21:15:31Z
url: https://github.com/BurntSushi/ripgrep/pull/139
synced_at: 2026-01-12T18:23:12Z
```

# Make the repo a Homebrew Tap

---

_@moshen_

Because there have been several comments on the builds provided by @BurntSushi [having `simd-accel` enabled](https://github.com/BurntSushi/ripgrep/issues/10#issuecomment-250612979), vs [HomeBrew using rust stable to build](https://github.com/Homebrew/homebrew-core/pull/5268).  I thought I would create this PR to enable the BurntSushi/ripgrep repo to be used as a Homebrew tap.  This would allow those who are tracking these releases (built with rust nightly?) to have easy updates, at least until the official Homebrew build can enable `simd-accel`.

Another aside, this PR removes the non-64-bit releases from the `Formula`.  This is because everything Mac OS 10.7 and newer has to be running on an x86_64 cpu.  If you want to continue supporting non-64-bit installs, I can add it back to this PR.


---

_Merged by @BurntSushi on 2016-10-03 21:14_

---

_Closed by @BurntSushi on 2016-10-03 21:14_

---

_Comment by @BurntSushi on 2016-10-03 21:15_

This is awesome, thank you.

> Another aside, this PR removes the non-64-bit releases from the Formula. This is because everything Mac OS 10.7 and newer has to be running on an x86_64 cpu. 

I'm not a Mac user so I hadn't realized this. I'll also remove the binary from the Github releases too. (An easy decision, given that it has been causing problems lately.)


---
