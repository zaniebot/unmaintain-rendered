```yaml
number: 605
title: search summary at the end
type: issue
state: closed
author: sjLambda
labels:
  - duplicate
assignees: []
created_at: 2017-09-16T17:15:52Z
updated_at: 2017-09-17T12:23:18Z
url: https://github.com/BurntSushi/ripgrep/issues/605
synced_at: 2026-01-12T16:13:22Z
```

# search summary at the end

---

_@sjLambda_

How can I get a summary of the search at the end? 

For example, ag gives this:

264 matches
48 files contained matches
307 files searched
16778135 bytes searched
3.377348 seconds

Is there a way to get some or all such stats with rg?

---

_Label `duplicate` added by @BurntSushi on 2017-09-16 17:17_

---

_Comment by @BurntSushi on 2017-09-16 17:18_

dupe of #411 

(To answer your question, no, ripgrep doesn't have this. Use `rg foo | wc -l` or `rg -l foo | wc -l` or `rg -c foo` for now.)

---

_Comment by @sjLambda on 2017-09-17 07:54_

Then I get the stats and not the search hits.

---

_Comment by @BurntSushi on 2017-09-17 12:23_

@sjLambda It's a work around. That's why there's an issue open for it. Did you read it?

Anyway, this is clearly a duplicate, so I'm closing.

---

_Closed by @BurntSushi on 2017-09-17 12:23_

---
