```yaml
number: 432
title: Add short note on Windows Tips
type: pull_request
state: merged
author: DoumanAsh
labels: []
assignees: []
merged: true
base: master
head: win_tips
created_at: 2017-03-31T13:54:39Z
updated_at: 2017-04-09T12:32:41Z
url: https://github.com/BurntSushi/ripgrep/pull/432
synced_at: 2026-01-12T18:23:13Z
```

# Add short note on Windows Tips

---

_@DoumanAsh_

_No description provided._

---

_Review comment by @BurntSushi on `README.md`:406 on 2017-03-31 13:56_

I think this looks OK to me. Is there any way to put this function into a file that is automatically loaded whenever powershell starts?

cc @retep998

---

_@BurntSushi reviewed on 2017-03-31 13:56_

---

_@DoumanAsh reviewed on 2017-03-31 13:57_

---

_Review comment by @DoumanAsh on `README.md`:406 on 2017-03-31 13:57_

Ah yes, i add notes on that too

---

_Comment by @DoumanAsh on 2017-03-31 14:02_

I added note on how to use Powershell Profile

---

_@retep998 reviewed on 2017-03-31 16:58_

---

_Review comment by @retep998 on `README.md`:387 on 2017-03-31 16:58_

crated?

---

_@retep998 reviewed on 2017-03-31 16:59_

---

_Review comment by @retep998 on `README.md`:390 on 2017-03-31 16:59_

`it's` -> `its`

---

_@DoumanAsh reviewed on 2017-03-31 17:05_

---

_Review comment by @DoumanAsh on `README.md`:387 on 2017-03-31 17:05_

fixed

---

_@DoumanAsh reviewed on 2017-03-31 17:06_

---

_Review comment by @DoumanAsh on `README.md`:390 on 2017-03-31 17:06_

fixed

---

_@elirnm reviewed on 2017-04-01 05:48_

---

_Review comment by @elirnm on `README.md`:390 on 2017-04-01 05:48_

The location of the profile can be gotten just with `$profile`; no need for that complex command. And you can check to see if a profile exists with `test-path $profile`. You might want to link to the [official docs on profiles](https://technet.microsoft.com/en-us/library/bb613488(v=vs.85).aspx), which cover the various profile locations and have instructions on how to create one if it doesn't exist (`new-item -path $profile -itemtype file -force`).

---

_@DoumanAsh reviewed on 2017-04-01 06:20_

---

_Review comment by @DoumanAsh on `README.md`:390 on 2017-04-01 06:20_

Well, ok...

---

_Merged by @BurntSushi on 2017-04-09 12:32_

---

_Closed by @BurntSushi on 2017-04-09 12:32_

---

_Comment by @BurntSushi on 2017-04-09 12:32_

@DoumanAsh Thanks again!

---
