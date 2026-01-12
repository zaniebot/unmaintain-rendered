```yaml
number: 560
title: Refactor completion function
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: feature/complete-refactor
created_at: 2017-07-18T09:44:38Z
updated_at: 2017-07-18T15:58:57Z
url: https://github.com/BurntSushi/ripgrep/pull/560
synced_at: 2026-01-12T18:23:13Z
```

# Refactor completion function

---

_@okdana_

Hey again! I've been collecting issues left over from my previous effort and i think i've found as many as i can for now.

The biggest problem, which was carried over from the original, was that option completion was basically broken on zsh 5.1 (the latest version available in Ubuntu Xenial). But i also found some typos in option names, missing actions for options that take arguments, missing exclusion rules, &c.

In addition to fixing bugs i improved the `colorspec` handling slightly, and i added completion for sequences of types when given `--type-add <type>:include:`. And i re-ordered the options to match the `--help` output like i mentioned wanting to do before.

I tested this on zsh 5.1, zsh 5.2, and zsh 5.3 and it seems OK. Really hard to test properly though, so i'm sure i'll find something again....

---

_Review comment by @BurntSushi on `ci/test_complete.sh`:7 on 2017-07-18 11:00_

It seems like you could probably install zsh in the Travis CI config?

---

_@BurntSushi approved on 2017-07-18 11:02_

Awesome! As a Bash user, I might start to get jealous. :-)

And yeah, I appreciate that testing this stuff is hard. Fixing things as we go seems like the best course of action.

---

_Comment by @BurntSushi on 2017-07-18 11:03_

*sigh* It looks like Windows CI is failing due to an unrelated test failure. Merging this anyway.

---

_Merged by @BurntSushi on 2017-07-18 11:03_

---

_Closed by @BurntSushi on 2017-07-18 11:03_

---

_@okdana reviewed on 2017-07-18 15:58_

---

_Review comment by @okdana on `ci/test_complete.sh`:7 on 2017-07-18 15:58_

Oh, i wasn't sure it was an option. Knowing that it is, i'll probably do that the next time i need to make changes to the function.

---
