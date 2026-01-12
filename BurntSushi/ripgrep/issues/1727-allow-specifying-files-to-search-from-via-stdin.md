```yaml
number: 1727
title: "allow specifying files to search from via stdin, eg: `find / | rg pattern --stdin`"
type: issue
state: closed
author: timotheecour
labels:
  - duplicate
assignees: []
created_at: 2020-11-08T18:54:04Z
updated_at: 2020-11-09T12:28:08Z
url: https://github.com/BurntSushi/ripgrep/issues/1727
synced_at: 2026-01-12T16:13:24Z
```

# allow specifying files to search from via stdin, eg: `find / | rg pattern --stdin`

---

_@timotheecour_

#### Describe your feature request

I'd like to allow searching for a pattern in files specified via stdin, eg: `find / | rg pattern --stdin`
note that `find / | rg pattern -` is different, as it treats stdin as a file and searches for a pattern there.

The motivation is it allows separation of concerns, eg using some program to specify which files you're interested in searching from, and then asking ripgrep to search for content among those.


---

_Renamed from "allow specifying files to search from via stdin" to "allow specifying files to search from via stdin, eg: `find / | rg pattern --stdin`" by @timotheecour on 2020-11-08 18:54_

---

_Comment by @BurntSushi on 2020-11-09 12:28_

You can do this with xargs: `find / -print0 | xargs -0 rg pattern`.

See also #273. Which I've just re-opened.

---

_Closed by @BurntSushi on 2020-11-09 12:28_

---

_Label `duplicate` added by @BurntSushi on 2020-11-09 12:28_

---
