```yaml
number: 157
title: Always follow symlinks on explicit file arguments
type: pull_request
state: merged
author: isker
labels: []
assignees: []
merged: true
base: master
head: follow-explicit-args
created_at: 2016-10-09T02:44:13Z
updated_at: 2016-10-10T23:11:13Z
url: https://github.com/BurntSushi/ripgrep/pull/157
synced_at: 2026-01-12T18:23:12Z
```

# Always follow symlinks on explicit file arguments

---

_@isker_

Fixes #137.  I have no prior knowledge, but it looks like Args.walker() is the correct place to locate this...

Let me know if you think this should be documented anyplace.


---

_Merged by @BurntSushi on 2016-10-10 23:11_

---

_Closed by @BurntSushi on 2016-10-10 23:11_

---

_Comment by @BurntSushi on 2016-10-10 23:11_

This seems OK, thank you for fixing it.

I have several style nits, but I'm just going to merge them and fix them myself.


---
