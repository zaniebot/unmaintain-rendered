```yaml
number: 924
title: Move termcolor to separate repository?
type: issue
state: closed
author: LPGhatguy
labels:
  - question
assignees: []
created_at: 2018-05-21T19:10:47Z
updated_at: 2018-07-17T22:04:55Z
url: https://github.com/BurntSushi/ripgrep/issues/924
synced_at: 2026-01-12T16:13:22Z
```

# Move termcolor to separate repository?

---

_@LPGhatguy_

Termcolor is the coloring crate used by Cargo (since June, 2017), which indicates to me that it should be or become the default recommended terminal coloring crate for Rust. Termcolor also handles Windows the most robustly out of all of the terminal coloring crates available.

For discoverability, should termcolor move to a separate repository?

* Filing issues and PRs for termcolor specifically feels out-of-place in a large project like ripgrep
* It's not clear whether termcolor is intended to be used by other projects when it lives as a sub-folder
* It's hard to keep up with issues filed against termcolor when they're a tiny subset of ripgrep's
    * ie. GitHub's "Watch" feature would blow up my inbox

---

_Comment by @BurntSushi on 2018-05-21 19:21_

> which indicates to me that it should be or become the default recommended terminal coloring crate for Rust.

It might, but `termcolor`'s API is quite inconvenient compared to other terminal coloring APIs, and this is mostly because it handles Windows console coloring correctly, which is necessary for pre-Windows 10 support. If you only care about supporting Windows 10 or newer, then you have a lot more choices.

> For discoverability, should termcolor move to a separate repository?

Not sure. The issue volume for termcolor is quite low. The reason why it (and the other crates) are in the ripgrep repo is because it simplifies maintenance for me, which I attach a lot of value to.

I'm not sure that there's too much else left to do for termcolor anyway. It will never grow beyond very basic color/styling support. It's probably ripe for a 1.0 release.

---

_Label `question` added by @BurntSushi on 2018-05-21 19:22_

---

_Comment by @gilescope on 2018-06-20 16:45_

I was using ripgrep on the default powershell terminal which has a blue background. The red with blue doesn't work well. It got me thinking that term colors for error / warning / good probably don't work well for some colorblind users. It would be pretty cool if we could somehow support colorblind cli tools out of the box. Anyway maybe this is an argument for terminal coloring being separated so functionality like that can be solved in one place?

---

_Comment by @BurntSushi on 2018-06-20 16:48_

@gilescope Huh? `termcolor` is already a separate crate. It's just in this repo. Please see the ripgrep documentation on how to change the colors.

This issue should be about process and maintenance. Specific features are entirely orthogonal.

---

_Comment by @gilescope on 2018-06-20 17:13_

Good point. And the more I think of color blindness support, the more I think its better solved once at the OS level like iOS/Android do.

---

_Closed by @BurntSushi on 2018-07-17 21:46_

---

_Comment by @BurntSushi on 2018-07-17 22:04_

I've moved `termcolor` to its own repository: https://github.com/BurntSushi/termcolor

My intent is to release 1.0 imminently.

---
