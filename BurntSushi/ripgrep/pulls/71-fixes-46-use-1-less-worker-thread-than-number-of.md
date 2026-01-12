```yaml
number: 71
title: "[Fixes #46] Use 1 less worker thread than number of threads"
type: pull_request
state: merged
author: catchmrbharath
labels: []
assignees: []
merged: true
base: master
head: issue46
created_at: 2016-09-25T00:31:51Z
updated_at: 2016-09-25T19:02:43Z
url: https://github.com/BurntSushi/ripgrep/pull/71
synced_at: 2026-01-12T18:23:12Z
```

# [Fixes #46] Use 1 less worker thread than number of threads

---

_@catchmrbharath_

The main thread does directory traversal. Hence
number of threads = main Thread + number of worker threads.
We should have atleast one worker thread.


---

_Comment by @BurntSushi on 2016-09-25 00:39_

Could you add "Fixes #46" to the commit message so Github links them? Otherwise this looks good, thanks!


---

_Merged by @BurntSushi on 2016-09-25 19:02_

---

_Closed by @BurntSushi on 2016-09-25 19:02_

---

_Comment by @BurntSushi on 2016-09-25 19:02_

Thanks!


---
