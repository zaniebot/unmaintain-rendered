```yaml
number: 143
title: Fix brew formula name to not conflict with core
type: pull_request
state: merged
author: moshen
labels: []
assignees: []
merged: true
base: master
head: change-brew-formula-name
created_at: 2016-10-03T21:44:12Z
updated_at: 2016-10-04T16:28:11Z
url: https://github.com/BurntSushi/ripgrep/pull/143
synced_at: 2026-01-12T18:23:12Z
```

# Fix brew formula name to not conflict with core

---

_@moshen_

Since the homebrew-core formula was accepted, we should differentiate
the prebuilt formula available in this tap

Only after my previous PR was accepted (and I tried it again), did I notice that there may be an issue with conflicting formula names:

![image](https://cloud.githubusercontent.com/assets/168513/19055204/7049a34e-8988-11e6-9832-c20d294e146c.png)

This is failing because the cache I have from installing the `homebrew-core` version of ripgrep doesn't match the sha of the prebuilt download.


---

_Comment by @BurntSushi on 2016-10-03 21:52_

Is `-prebuilt` an idiom that the Homebrew ecosystem uses? If not, is there one?


---

_Comment by @moshen on 2016-10-03 22:02_

Good question.  There is really only `bottle` (but you aren't distributing `bottle`s) from what I can tell.  This actually seems like more of a problem with Homebrew since it doesn't namespace the cache by tap name.


---

_Comment by @BurntSushi on 2016-10-03 22:15_

Hmm. I would like to make sure we get this name right. For example, I can imagine being convinced that the tap should compile from source instead of distributing prebuilt binaries. However, that's unlikely to happen unless something like `rustup` gets packaged because we'd need a nightly compiler to enable SIMD.

In Archlinux, we'd call this `ripgrep-git`, but that's not accurate because this tap doesn't track master. We also have things like `ripgrep-bin`, but that has the same problem as `ripgrep-prebuilt`.

Maybe I'm overthinking this. (I do like `ripgrep-bin` better than `ripgrep-prebuilt` though.)


---

_Comment by @moshen on 2016-10-04 03:27_

It appears that [multirust](https://github.com/Homebrew/homebrew-core/blob/d4109a06e6b3957a7091ced6de1dd9d4de801952/Formula/multirust.rb) is available (which uses rustup?).  However, there are no formula which depend on it.. so I'm not sure if that's allowed.  [I have a change that is using it to build with rust nightly](https://github.com/Homebrew/homebrew-core/pull/5563).  I guess this PR could just hang out depending on what happens there.  If the Homebrew PR is accepted and bottles created from the nightly build, I can revise this PR further.


---

_Merged by @BurntSushi on 2016-10-04 10:49_

---

_Closed by @BurntSushi on 2016-10-04 10:49_

---

_Comment by @BurntSushi on 2016-10-04 10:49_

`multirust` is on the way out unfortunately (to be replaced by `rustup`). The Homebrew folks probably made the right call by sticking with `rust`. In general, we really shouldn't be packaging nightly Rust at all. It just so happens that `ripgrep` benefits from it. I realize it sucks, but it's a good motivator for us (the Rust community) to get SIMD stabilized.

With that in mind, I'm just going to accept this and call it done. Thanks so much for straightening this out!


---

_Comment by @moshen on 2016-10-04 16:28_

Gotcha.  No problem!  Thanks for ripgrep! 🍻 


---
