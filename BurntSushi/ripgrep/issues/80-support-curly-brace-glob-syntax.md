```yaml
number: 80
title: Support curly brace glob syntax
type: issue
state: closed
author: SevereOverfl0w
labels:
  - enhancement
assignees: []
created_at: 2016-09-25T09:02:25Z
updated_at: 2016-09-25T21:28:49Z
url: https://github.com/BurntSushi/ripgrep/issues/80
synced_at: 2026-01-12T18:23:11Z
```

# Support curly brace glob syntax

---

_@SevereOverfl0w_

Supporting globbing like `{a,b}` would be extremely useful for "or" based usage: `*.min.{css,js}`, I imagine this would also be significantly more performant, particularly when using 7/8 separate globs instead of one big or glob.


---

_Comment by @BurntSushi on 2016-09-25 12:57_

It actually won't be more performant. Globs are already compiled down to a single finite state machine.

In any case, yes, we should support this syntax.


---

_Label `enhancement` added by @BurntSushi on 2016-09-25 19:05_

---

_Closed by @BurntSushi on 2016-09-25 21:28_

---

_Comment by @BurntSushi on 2016-09-25 21:28_

This was fun. All done!


---
