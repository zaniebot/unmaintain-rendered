```yaml
number: 121
title: if --color always, always print with color, even when --vimgrep is given
type: pull_request
state: merged
author: lilydjwg
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2016-09-28T03:55:53Z
updated_at: 2016-09-29T20:49:48Z
url: https://github.com/BurntSushi/ripgrep/pull/121
synced_at: 2026-01-12T18:23:12Z
```

# if --color always, always print with color, even when --vimgrep is given

---

_@lilydjwg_

_No description provided._

---

_@BurntSushi reviewed on 2016-09-28 11:05_

---

_Review comment by @BurntSushi on `src/args.rs`:363 on 2016-09-28 11:05_

I think you can change the `else` in this expression to be `false` now, right?


---

_@lilydjwg reviewed on 2016-09-28 12:13_

---

_Review comment by @lilydjwg on `src/args.rs`:363 on 2016-09-28 12:13_

Yes, I've changed that.


---

_Merged by @BurntSushi on 2016-09-29 20:49_

---

_Closed by @BurntSushi on 2016-09-29 20:49_

---

_Comment by @BurntSushi on 2016-09-29 20:49_

Thanks!


---
