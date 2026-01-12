```yaml
number: 250
title: Re-add zsh types
type: pull_request
state: merged
author: SimenB
labels: []
assignees: []
merged: true
base: master
head: zsh-types
created_at: 2016-11-25T08:54:44Z
updated_at: 2016-11-25T13:48:45Z
url: https://github.com/BurntSushi/ripgrep/pull/250
synced_at: 2026-01-12T18:23:12Z
```

# Re-add zsh types

---

_@SimenB_

Seems like #197 got lost in #202 

I added the missing `.zlogout`, and also search the ones without a leading `.` (which should be in `/etc`). Is there syntax for making a char optional (like `?` in regex)?
The entries are sorted in the order they're read by zsh, with the addition of a `*.zsh` at the end

---

_Merged by @BurntSushi on 2016-11-25 13:13_

---

_Closed by @BurntSushi on 2016-11-25 13:13_

---

_Comment by @BurntSushi on 2016-11-25 13:15_

Oh interesting! Not sure how they got lost, sorry about that!

In globs, I don't think there is an equivalent to `?` from regexes. You need to use some other syntax like `{foo,bar}`. ... But the way you wrote it here is better because it's easier for the glob match optimizer to see the literals. :-)

---

_Branch deleted on 2016-11-25 13:48_

---
