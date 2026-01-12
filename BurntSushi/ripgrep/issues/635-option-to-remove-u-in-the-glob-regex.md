```yaml
number: 635
title: Option to remove (?-u) in the glob.regex()?
type: issue
state: closed
author: jakwings
labels: []
assignees: []
created_at: 2017-10-11T01:53:15Z
updated_at: 2017-10-11T10:06:13Z
url: https://github.com/BurntSushi/ripgrep/issues/635
synced_at: 2026-01-12T16:13:22Z
```

# Option to remove (?-u) in the glob.regex()?

---

_@jakwings_

While working on sharkdp/fd#96, I found this: https://github.com/BurntSushi/ripgrep/blob/12ffcb4296e0c2068797daea6ed88885b060d916/globset/src/glob.rs#L596

Is there any reason for not having an option to allow `(?u)`?

---

_Comment by @BurntSushi on 2017-10-11 09:34_

Yes. Globs match paths, which aren't always UTF-8.

---

_Comment by @jakwings on 2017-10-11 10:06_

Ah, right. Maybe I should fix the "UTF8-aware matching" downstream. Closing for now.

---

_Closed by @jakwings on 2017-10-11 10:06_

---
