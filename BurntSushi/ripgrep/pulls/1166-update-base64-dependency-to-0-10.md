```yaml
number: 1166
title: Update base64 dependency to 0.10
type: pull_request
state: closed
author: silwol
labels: []
assignees: []
base: master
head: master
created_at: 2019-01-19T12:05:48Z
updated_at: 2019-01-19T15:56:08Z
url: https://github.com/BurntSushi/ripgrep/pull/1166
synced_at: 2026-01-12T18:23:13Z
```

# Update base64 dependency to 0.10

---

_@silwol_

_No description provided._

---

_@BurntSushi requested changes on 2019-01-19 12:44_

Could you also run `cargo update -p base64` in the root of the repo? Otherwise, subsequent  builds will just update the lock file anyway.

---

_Comment by @BurntSushi on 2019-01-19 15:56_

I ended up doing this in #1168 as part of a larger update. Thanks!

---

_Closed by @BurntSushi on 2019-01-19 15:56_

---
