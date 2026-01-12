```yaml
number: 1459
title: ripgrep git trees similar to git grep
type: issue
state: closed
author: dmitris
labels:
  - question
assignees: []
created_at: 2020-01-14T14:06:32Z
updated_at: 2024-05-15T03:41:21Z
url: https://github.com/BurntSushi/ripgrep/issues/1459
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep git trees similar to git grep

---

_@dmitris_

Possibly a silly question - would it be possible (or feasible) to extend ripgrep to be able to search in git trees (like other branches / commits) similar to how `git grep` does it?  I really like the tool, its speed, set of options etc. - but need to be able to search other branches / all commits (like https://github.com/dxa4481/truffleHog does).


---

_Comment by @BurntSushi on 2020-01-14 14:08_

It's a neat idea, but I think it's definitively outside the scope of ripgrep. The additional flags required for such a rich feature alone would, I'm guessing, probably increase ripgrep's surface area by a significant amount. However, someone could use ripgrep's libraries to build a new command line tool dedicated to the purpose. That seems like a better approach to me.

---

_Closed by @BurntSushi on 2020-01-14 14:08_

---

_Label `question` added by @BurntSushi on 2020-01-14 14:08_

---

_Comment by @nyurik on 2024-05-15 03:41_

I ran into a similar issue - doing a global search for something, only to realize it was in an older branch (took me multiple hours!).  I honestly wish there was a way for `rg` to even just give an indication that the needed content is gziped (?) inside the git repo - so that I could start searching its content using other tools...

---
