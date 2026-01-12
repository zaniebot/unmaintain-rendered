```yaml
number: 515
title: "Q&A: Should `-F` ignore all regular expression characters, including `$` and `^`?"
type: issue
state: closed
author: ghost
labels:
  - question
assignees: []
created_at: 2017-06-14T14:27:22Z
updated_at: 2017-06-14T15:47:09Z
url: https://github.com/BurntSushi/ripgrep/issues/515
synced_at: 2026-01-12T16:13:22Z
```

# Q&A: Should `-F` ignore all regular expression characters, including `$` and `^`?

---

_@ghost_

Recently I was looking for known values in a codebase that contain all kinds of characters, including `$`.  `-F` was used for this assuming this searches for 'fixed strings' in the sense that they are just a string literal. However, `$` is definitely interpreted by the regex engine, and needs to be escaped via `\$`.

Is this intended, or is this a bug?

Thank you,
Sebastian

PS: Please apologise if this is not the right forum for questions like these.

---

_Comment by @BurntSushi on 2017-06-14 14:41_

Could you please provide a reproducible example that demonstrates this behavior? It's pretty hard to help you otherwise. For example, things seem to be working correctly for me:

```
$ cat test
foo$bar
$ rg 'foo$' test
$ rg -F 'foo$' test
1:foo$bar
```

---

_Label `question` added by @BurntSushi on 2017-06-14 14:41_

---

_Comment by @ghost on 2017-06-14 15:46_

Thanks a lot for your swift reply! 
Also I am super sorry to have wasted your time ...!
In the shell, I managed to use double-quotes instead of single-quotes.

---

_Closed by @ghost on 2017-06-14 15:46_

---
