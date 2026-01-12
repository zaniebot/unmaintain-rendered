```yaml
number: 219
title: adding .pdf filetype to available types.
type: pull_request
state: merged
author: theamazingfedex
labels: []
assignees: []
merged: true
base: master
head: adding-pdf-filetype
created_at: 2016-11-03T21:46:41Z
updated_at: 2016-11-04T00:46:07Z
url: https://github.com/BurntSushi/ripgrep/pull/219
synced_at: 2026-01-12T18:23:12Z
```

# adding .pdf filetype to available types.

---

_@theamazingfedex_

_No description provided._

---

_Comment by @BurntSushi on 2016-11-03 21:47_

Uh, could you explain the use case for this? PDF is a binary format. It feels weird to watch to search it. ripgrep should, in most cases, detect the binary format and skip it altogether.


---

_Comment by @theamazingfedex on 2016-11-03 21:59_

I'm not entirely sure what the exact case was, but I kept encountering situations when trying to search through my work repository that inundated me with results from a .pdf file. I figure that if a user does end up enabling binary searching, then perhaps it would be useful to include some of the most common binary filetypes.


---

_Comment by @BurntSushi on 2016-11-03 22:10_

> but I kept encountering situations when trying to search through my work repository that inundated me with results from a .pdf file

Was this happening with ripgrep?

> I figure that if a user does end up enabling binary searching, then perhaps it would be useful to include some of the most common binary filetypes.

That's a good point.


---

_Merged by @BurntSushi on 2016-11-04 00:46_

---

_Closed by @BurntSushi on 2016-11-04 00:46_

---
