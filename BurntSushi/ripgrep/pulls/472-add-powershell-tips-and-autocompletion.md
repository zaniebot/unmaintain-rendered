```yaml
number: 472
title: Add Powershell tips and autocompletion instructions
type: pull_request
state: merged
author: elirnm
labels: []
assignees: []
merged: true
base: master
head: powershell-info
created_at: 2017-05-04T04:40:03Z
updated_at: 2017-05-08T23:23:48Z
url: https://github.com/BurntSushi/ripgrep/pull/472
synced_at: 2026-01-12T18:23:13Z
```

# Add Powershell tips and autocompletion instructions

---

_@elirnm_

Updated the readme with tips on piping non-ASCII content (the cause/fix for https://github.com/BurntSushi/ripgrep/issues/471) and on resetting colors after `ctrl+c` in Powershell (because `color` isn't supported), plus instructions for using the Powershell autocompletion file (which is dependent on ripgrep updating to clap 2.23.2, see https://github.com/BurntSushi/ripgrep/issues/445 and https://github.com/kbknapp/clap-rs/issues/931).

---

_Comment by @BurntSushi on 2017-05-04 10:56_

@elirnm This looks great, thanks so much for writing it up! Would you mind formatting the additions in the same style as the rest of the README? I think in this case that's making sure the text wraps at 79 columns. And then please squash everything down to one commit. (I can do this for you if you like when I get around to it.)

---

_Comment by @elirnm on 2017-05-04 23:59_

@BurntSushi I formatted and squashed the changes.

---

_Merged by @BurntSushi on 2017-05-08 23:23_

---

_Closed by @BurntSushi on 2017-05-08 23:23_

---

_Comment by @BurntSushi on 2017-05-08 23:23_

@elirnm Thanks so much!

---
