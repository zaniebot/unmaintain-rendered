```yaml
number: 1515
title: Ripgrep among mixed encoded files
type: issue
state: closed
author: y-kuun
labels:
  - wontfix
assignees: []
created_at: 2020-03-14T17:11:19Z
updated_at: 2020-03-14T17:16:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1515
synced_at: 2026-01-12T16:13:23Z
```

# Ripgrep among mixed encoded files

---

_@y-kuun_

It seems that rigprep can handle files with the same encoding system only, e.g., `-E gb18030` only grep files encoded with gb18030.

My question is how can I grep the content that I want among files encoding with gb18030 and UTF-8 under one certain directory.




---

_Comment by @BurntSushi on 2020-03-14 17:16_

You can't. Or at least, I don't see any simple UX to enable such behavior. Sorry.

---

_Closed by @BurntSushi on 2020-03-14 17:16_

---

_Label `wontfix` added by @BurntSushi on 2020-03-14 17:16_

---
