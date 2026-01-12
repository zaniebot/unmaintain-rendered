```yaml
number: 24
title: "rename the `-r` option"
type: issue
state: closed
author: ChrisJefferson
labels: []
assignees: []
created_at: 2016-09-23T18:29:50Z
updated_at: 2021-04-02T15:43:29Z
url: https://github.com/BurntSushi/ripgrep/issues/24
synced_at: 2026-01-12T16:13:21Z
```

# rename the `-r` option

---

_@ChrisJefferson_

I just got very confused for quite a while, as my fingers tend to just naturally type `-r` which wanting a recursive search sometimes. The current `-r` does something (to me) really confusing (I'm not really sure what it would be useful for, but I'm sure it was added for a reason). However, it is (in my opinion) worth considering leaving `-r` unused, as it's such a common option people give to `grep` (then at least they get an error message).


---

_Comment by @BurntSushi on 2016-09-24 02:16_

The `-r` flag is short for `--replace`, which pretty much does what it says on the tin: it replaces each match with the value given to the flag, expanding capturing groups if necessary.

Can you suggest an alternative shorthand for `--replace`?


---

_Comment by @durka on 2016-09-24 02:25_

You could use `-s` (like `sed s/.../.../`). But unfortunately `s` isn't one of the letters in `replace` :/


---

_Closed by @BurntSushi on 2016-09-24 23:26_

---

_Comment by @BurntSushi on 2016-09-24 23:27_

I'm sympathetic to this request. Muscle memory is tricky. But I really like `-r` as a short hand for `--replace`.

I'd prefer we try to fix this by making it clearer that `rg` will do recursive search. I added some clarification to the output of `--help`.


---

_Comment by @cmgbhm on 2021-04-02 15:03_

@BurntSushi Maybe at least include this warning in the README.md?.   I found ripgrep a year or two ago (grep->ack->silversearcher).... 

grep -r (recursive) memory  getting confused  -> ends up nuking lots of files.  Fortunately, it was a fresh repo checkout but even figuring out what the glob actually replaced would have been a painful day in another directory :)

---

_Comment by @BurntSushi on 2021-04-02 15:10_

> Maybe at least include this warning in the README.md?

It is: "ripgrep is a line-oriented search tool that recursively searches your current directory for a regex pattern."

> grep -r (recursive) memory getting confused -> ends up nuking lots of files.

What? ripgrep doesn't nuke anything. I don't know what you're talking about. When complaining, please be **specific.**

---

_Comment by @cmgbhm on 2021-04-02 15:43_

Ack @BurntSushi;   You're correct and it's my bad.   I only saw it on the OUTPUT side and was bewildered :)

---
