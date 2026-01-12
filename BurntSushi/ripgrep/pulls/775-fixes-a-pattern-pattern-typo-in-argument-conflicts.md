```yaml
number: 775
title: "fixes a PATTERN->pattern typo in argument conflicts"
type: pull_request
state: merged
author: kbknapp
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2018-02-05T16:22:46Z
updated_at: 2018-02-05T16:42:58Z
url: https://github.com/BurntSushi/ripgrep/pull/775
synced_at: 2026-01-12T18:23:13Z
```

# fixes a PATTERN->pattern typo in argument conflicts

---

_@kbknapp_

Found this while testing kbknapp/clap-rs#868


---

_Comment by @BurntSushi on 2018-02-05 16:36_

Lovely! Thanks for finding this. :-)

---

_Merged by @BurntSushi on 2018-02-05 16:37_

---

_Closed by @BurntSushi on 2018-02-05 16:37_

---

_Comment by @okdana on 2018-02-05 16:37_

Oops

---

_Branch deleted on 2018-02-05 16:37_

---

_Comment by @BurntSushi on 2018-02-05 16:42_

@okdana This one was actually my fault, not yours! I switched the identifier name to `pattern` (was `PATTERN`) but kept the argument value name as `PATTERN` (which is what is shown in the `--help` output) and forgot to update a place that was using `PATTERN`. :-)

---
