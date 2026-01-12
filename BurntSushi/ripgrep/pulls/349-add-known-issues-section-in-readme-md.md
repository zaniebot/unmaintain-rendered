```yaml
number: 349
title: "Add \"Known issues\" section in README.md"
type: pull_request
state: merged
author: jmcomets
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2017-02-04T10:12:48Z
updated_at: 2017-03-08T15:18:24Z
url: https://github.com/BurntSushi/ripgrep/pull/349
synced_at: 2026-01-12T18:23:12Z
```

# Add "Known issues" section in README.md

---

_@jmcomets_

Also document that ctrl-c doesn't restore the termcolor (#347).

---

_Review comment by @BurntSushi on `README.md` on 2017-02-18 16:41_

I don't think "unsolvable deadlock" is an accurate summary. I'd rather just say "see this PR and this issue for more details."

Also, can you please wrap this to 80 columns and find a way to shorten the header? Please use a style that's consistent with the rest of the README.

---

_@BurntSushi requested changes on 2017-02-18 16:41_

---

_Comment by @BurntSushi on 2017-02-18 17:15_

Could you also please:

1. Squash this down to a single commit when you're ready.
2. Add `Fixes #347` to the bottom of your commit message.

---

_Merged by @BurntSushi on 2017-03-08 15:18_

---

_Closed by @BurntSushi on 2017-03-08 15:18_

---

_Comment by @BurntSushi on 2017-03-08 15:18_

Thanks!

---
