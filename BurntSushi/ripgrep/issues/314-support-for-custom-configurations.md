```yaml
number: 314
title: Support for custom configurations
type: issue
state: closed
author: kenny1847
labels: []
assignees: []
created_at: 2017-01-12T09:56:06Z
updated_at: 2017-01-12T15:11:04Z
url: https://github.com/BurntSushi/ripgrep/issues/314
synced_at: 2026-01-12T16:13:21Z
```

# Support for custom configurations

---

_@kenny1847_

This relates to https://github.com/BurntSushi/ripgrep/issues/196.
There is going thread about persistent global config, but, on the other hand, as for me, in many cases I need custom .rgrc for custom project. And it will be great pros, because anyone invited to project won't has to write own aliases or anything what he/she likes.

---

_Comment by @BurntSushi on 2017-01-12 15:09_

I don't think we need two tickets tracking this. If directory specific configs are desirable, then that should probably be part of the initial design, since I expect it will add an extra layer of complexity.

---

_Closed by @BurntSushi on 2017-01-12 15:11_

---
