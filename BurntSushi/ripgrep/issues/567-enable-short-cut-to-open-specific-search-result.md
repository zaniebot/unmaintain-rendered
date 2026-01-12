```yaml
number: 567
title: Enable short-cut to open specific search result in editor
type: issue
state: closed
author: robdmc
labels:
  - question
assignees: []
created_at: 2017-07-27T14:14:53Z
updated_at: 2017-07-27T14:23:31Z
url: https://github.com/BurntSushi/ripgrep/issues/567
synced_at: 2026-01-12T16:13:22Z
```

# Enable short-cut to open specific search result in editor

---

_@robdmc_

I constantly use the `sack` tool ( https://github.com/sampson-chen/sack ) to provide a nice wrapper around my silversearcher results.  It allows me to quickly open a file with vim at the exact location of the search result.  As you can imagine, this saves tons of time when exploring a large code base.  I would love to see this capability in ripgrep.  I recently discovered a similar project implemented in go  https://github.com/zph/go-sack.  I'm not sure how difficult it would be to add this capability to ripgrep, but it would greatly improve its usefulness.

---

_Renamed from "Allow editing shortcuts for search results like the sack project" to "Enable short-cut to open specific search result in editor" by @robdmc on 2017-07-27 14:16_

---

_Comment by @BurntSushi on 2017-07-27 14:22_

ripgrep is certainly not going to grow `sack` functionality on its own. It seems like it should be added as a wrapper tool like it is for the silver searcher. Adding editor specific functionality to ripgrep doesn't seem like something I'd be willing to maintain.

---

_Label `question` added by @BurntSushi on 2017-07-27 14:22_

---

_Comment by @robdmc on 2017-07-27 14:23_

Thanks for the quick reply.  Great work with ripgrep.

---

_Closed by @robdmc on 2017-07-27 14:23_

---
