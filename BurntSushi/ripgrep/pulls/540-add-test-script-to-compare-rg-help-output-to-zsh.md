```yaml
number: 540
title: "Add test script to compare `rg --help` output to zsh completion function"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: feature/test_complete
created_at: 2017-07-06T02:33:29Z
updated_at: 2017-07-06T23:00:37Z
url: https://github.com/BurntSushi/ripgrep/pull/540
synced_at: 2026-01-12T18:23:13Z
```

# Add test script to compare `rg --help` output to zsh completion function

---

_@okdana_

Per our discussion in #536: Here's something to hopefully prevent the zsh completion function from getting out-of-synch with the actual app.

I used a slightly different strategy from the one i mentioned before, but it's the same basic idea — pull all of the options out of the `--help` output, pull all of the options out of the completion function, compare the two. I tried to address your concerns about false positives/negatives as best i could; i've documented the assumptions i make in the script, and i think it should be OK as long as nobody does anything bizarre.

I've tested this in `dash`, `bash`, and `zsh`. It also works in `ksh93` if you make `local` an alias for `typeset`... but you use `local` in your other scripts so i assume `ksh` compatibility isn't a big concern.

Anyway, it turns out actually that the `-f` option is missing from the completion function, which is fortuitous i guess because it seems to demonstrate that the script behaves as expected! Here's the output:

```
heartswap:ripgrep % ci/test_complete.sh
zsh completion options differ from `--help` options:
--- `rg --help`
+++ complete/_rg
@@ -77,5 +77,4 @@
 -c
 -e
--f
 -g
 -h
```

Unless you have concerns with it i think i'm happy with the script itself. I'm not sure how to go about integrating it with your other tests though. Should i just add it to `appveyor.yml` or does it belong somewhere else?

---

_Review comment by @BurntSushi on `ci/test_complete.sh`:8 on 2017-07-06 10:51_

We probably can't do this exact thing. This would at, the very least, depend on ripgrep being built before these tests are run. I think we could probably do it by using a debug build, but right now, a release build is not built in CI. It might be easier to just use `grep`. :P (I think you could set `rg=grep` and it will just work.)

---

_Review comment by @BurntSushi on `ci/test_complete.sh`:8 on 2017-07-06 10:51_

Oh hmm I see you're using the `--replace` flag. If you just change `rg` to `target/${TARGET}/debug/rg`, then I think that should be OK.

---

_@BurntSushi requested changes on 2017-07-06 10:55_

This looks pretty good! And yeah, I don't think I care too much about korn shell compatibility. :-) (Note that I am not the originator of these shell scripts.) If you wanted to make this `#!/bin/bash` to be more precise, then that's fine!

To add this to CI, I think it needs to go in `ci/script.sh`, which is run by Travis. Once you do that, it should be testable by pushing to this PR.

---

_@okdana reviewed on 2017-07-06 16:10_

---

_Review comment by @okdana on `ci/test_complete.sh`:8 on 2017-07-06 16:10_

Yeah, i could easily have replicated most of the find/replace stuff with `grep` and/or `sed`, but i figured that since i needed `rg` for the `--help` output anyway i might as well just use it for everything

---

_Comment by @okdana on 2017-07-06 16:15_

Updated the `rg` path and added the script to `script.sh` — seems to have worked!

Would you like me to add completion for `-f` as part of this PR, or should i make another one?

---

_Comment by @BurntSushi on 2017-07-06 16:40_

> Would you like me to add completion for -f as part of this PR, or should i make another one?

Whichever is most convenient for you! I'm find smushing it into this one via a separate commit.

---

_Comment by @okdana on 2017-07-06 19:09_

Done, seems like everything's passing now!

---

_Renamed from "[WIP] Add test script to compare `rg --help` output to zsh completion function" to "Add test script to compare `rg --help` output to zsh completion function" by @okdana on 2017-07-06 22:19_

---

_@BurntSushi approved on 2017-07-06 23:00_

---

_Comment by @BurntSushi on 2017-07-06 23:00_

@okdana You're awesome. :-) Thanks!

---

_Merged by @BurntSushi on 2017-07-06 23:00_

---

_Closed by @BurntSushi on 2017-07-06 23:00_

---
