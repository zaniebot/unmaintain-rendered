```yaml
number: 2453
title: Update rg.1.txt.tpl
type: pull_request
state: closed
author: ruppde
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2023-03-14T18:04:11Z
updated_at: 2023-07-08T22:52:51Z
url: https://github.com/BurntSushi/ripgrep/pull/2453
synced_at: 2026-01-12T18:23:14Z
```

# Update rg.1.txt.tpl

---

_@ruppde_

Clarify arguments in config files

---

_Review comment by @BurntSushi on `doc/rg.1.txt.tpl`:189 on 2023-03-14 18:09_

This should follow the style consistent with the surrounding content and be wrapped to 79 columns (inclusive).

Also, this phrasing kind of just seems to confuse things even more from my perspective. The point is that something like `-j 4` would be _one_ argument when put on the same line. So it needs to be:

```
-j
4
```

or

```
-j4
```

So I'm not sure if this is the right phrasing here...

---

_@BurntSushi requested changes on 2023-03-14 18:09_

Whatever changes we make here should also go in the GUIDE too.

---

_@ruppde reviewed on 2023-03-14 18:20_

---

_Review comment by @ruppde on `doc/rg.1.txt.tpl`:189 on 2023-03-14 18:20_

Fixed it

The usual perspective is "I want -j 4 to go into the config file" but the text was written the other way around: How the config file is parsed.

---

_Comment by @BurntSushi on 2023-07-07 14:46_

I ended up going with different wording. The fact that the docs describe how the config file is parsed is intentional and I don't want to change that. I also feel like the examples in the man page already clarified your use case pretty well. But I added one more example.

---

_Label `rollup` added by @BurntSushi on 2023-07-07 14:46_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
