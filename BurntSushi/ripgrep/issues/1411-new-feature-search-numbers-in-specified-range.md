```yaml
number: 1411
title: "New feature: search numbers in specified range"
type: issue
state: closed
author: mechatroner
labels: []
assignees: []
created_at: 2019-10-27T03:37:43Z
updated_at: 2019-10-27T09:51:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1411
synced_at: 2026-01-12T16:13:23Z
```

# New feature: search numbers in specified range

---

_@mechatroner_

I understand that searching numbers in specific range is not related to regular expressions, but from the other hand this is a search problem - exactly what ripgrep is about.
So it would be cool to search numbers: integers/floats in specific range passed as argument to ripgrep, e.g.  `1000000-1048576`. ripgrep can also calculate expressions like `1024 * 1024` in searched files and return them as matched lines for this number search mode. This is useful when someone tries to find a numeric constant (only approximate value is known) that is supposed to be somewhere in the code.

---

_Comment by @BurntSushi on 2019-10-27 09:51_

It's an interesting feature, but I think it's beyond the scope for ripgrep. You really have two feature request here:

1. It shouldn't be too hard to write a script that converts a number range to a regex. Then just feed that to ripgrep.
2. Committing the value of arbitrary mathematical expressions in code is definitely well beyond the scope of ripgrep. And moreover is not at all consistent with its line oriented nature. If you want semantic understanding of what you're searching, then you'll need to look for another tool.

---

_Closed by @BurntSushi on 2019-10-27 09:51_

---
