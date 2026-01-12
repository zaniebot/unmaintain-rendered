```yaml
number: 1502
title: add engine option to easily switch between regexp engines
type: pull_request
state: closed
author: pierreN
labels: []
assignees: []
base: master
head: feature/engine
created_at: 2020-02-27T16:01:55Z
updated_at: 2020-03-15T13:52:53Z
url: https://github.com/BurntSushi/ripgrep/pull/1502
synced_at: 2026-01-12T18:23:13Z
```

# add engine option to easily switch between regexp engines

---

_@pierreN_

This is the commits corresponding to #1488

I tried to not change any default behavior of the options.

If I've done things correctly, the only thing that should change is when using the "auto-hybrid-regex" option without pcre2 feature enabled, an additional error message stating that pcre2 is not enabled should appear.

This is a follow up on : https://github.com/BurntSushi/ripgrep/pull/1499

Thanks !

---

_Comment by @pierreN on 2020-02-28 17:31_

Just noticed that `suggest_pcre` when `pcre2` is not enabled was not next to its enabled counterpart, so amended the first commit to reflect that.

---

_Comment by @pierreN on 2020-03-06 14:18_

Ping, any update on this by any chance ? Thanks.

---

_Closed by @BurntSushi on 2020-03-15 13:39_

---

_Branch deleted on 2020-03-15 13:52_

---
