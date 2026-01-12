```yaml
number: 350
title: Make it possible for termcolor to write to stderr (take 2)
type: pull_request
state: merged
author: pkgw
labels: []
assignees: []
merged: true
base: master
head: pr-stderr-take-2
created_at: 2017-02-05T17:19:49Z
updated_at: 2017-02-19T16:27:50Z
url: https://github.com/BurntSushi/ripgrep/pull/350
synced_at: 2026-01-12T18:23:12Z
```

# Make it possible for termcolor to write to stderr (take 2)

---

_@pkgw_

Pursuant to the discussion in #324. The new implementation breaks the `termcolor` API by renaming the `Stdout` type to `StandardStream`, which now has two constructors `stdout()` and `stderr()` rather than a single `new()`. `StdoutLock` is changed analogously.

Implementation was very straightforward.

---

_@BurntSushi requested changes on 2017-02-09 19:36_

@pkgw OK, this looks much better, thank you. :-) And a major thanks for staying patient with me!

I think this looks good to go, but I would like to see this squashed down to one commit before merging. :-)

---

_Comment by @pkgw on 2017-02-09 19:48_

Aw, I kind of liked how the commits had a very clear logical progression through the needed changes, but it's really no big deal to me. Squashed and re-pushed.

---

_Comment by @BurntSushi on 2017-02-09 19:54_

@pkgw Yeah, one commit makes things easier to revert or consider one group of logical changes. Ideally, every commit builds and passes tests. This is also important since I've switched to a "no merges ever" model a couple months ago.

---

_Comment by @BurntSushi on 2017-02-09 19:56_

@pkgw Errm, OK, sorry I have a couple more nits:

1. Could you please add `[breaking-change]` to the commit message.
2. Could you also add `Fixes #324` to the commit message.

Thanks! (I really should get a `CONTRIBUTING.md` in place...)

---

_Comment by @pkgw on 2017-02-09 20:08_

OK, updated.

---

_@BurntSushi approved on 2017-02-09 20:10_

---

_Comment by @pkgw on 2017-02-10 00:38_

Passed CI 👍

---

_Merged by @BurntSushi on 2017-02-10 01:57_

---

_Closed by @BurntSushi on 2017-02-10 01:57_

---

_Comment by @BurntSushi on 2017-02-10 01:57_

Not sure why CI took so long, but now I'm tired from being sick and shoveling snow, and I have a rule never to do releases when I'm tired. Tomorrow. :-)

---

_Branch deleted on 2017-02-10 13:27_

---

_Comment by @BurntSushi on 2017-02-19 16:16_

Sorry it took so long, but this PR is on crates.io in `termcolor 0.3.0`. Thanks again! :-)

---

_Comment by @pkgw on 2017-02-19 16:20_

Was actually *just* about to inquire about a new release ... nice! I notice that the `termcolor` README.md still say that you should write

```
[dependencies]
termcolor = "0.1"
```

which should probably be updated to the new version, given the new features?

---

_Comment by @BurntSushi on 2017-02-19 16:27_

@pkgw Ah right of course! All set. :-)

---
