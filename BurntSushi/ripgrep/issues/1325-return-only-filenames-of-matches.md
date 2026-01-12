```yaml
number: 1325
title: Return only filenames of matches
type: issue
state: closed
author: Kyle-Thompson
labels: []
assignees: []
created_at: 2019-07-19T19:48:28Z
updated_at: 2019-07-19T20:29:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1325
synced_at: 2026-01-12T16:13:23Z
```

# Return only filenames of matches

---

_@Kyle-Thompson_

It would be useful to have an option that when set will cause rg to print only the names of the files that contain matches. I've run into situations before where it would be useful to say `vim -O $(rg --match-filenames-only keyword)`.

---

_Comment by @BurntSushi on 2019-07-19 20:19_

The `-l/--files-with-matches` is standard and has been available in ripgrep probably since its first release. Why can't you use that?

---

_Comment by @Kyle-Thompson on 2019-07-19 20:29_

Don't mind me, I'm just an idiot. Somehow missed that.

---

_Closed by @Kyle-Thompson on 2019-07-19 20:29_

---
