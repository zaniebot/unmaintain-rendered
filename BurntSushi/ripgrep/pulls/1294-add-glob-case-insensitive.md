```yaml
number: 1294
title: Add --glob-case-insensitive
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/glob-case-insensitive
created_at: 2019-06-06T17:39:55Z
updated_at: 2019-08-01T21:09:00Z
url: https://github.com/BurntSushi/ripgrep/pull/1294
synced_at: 2026-01-12T18:23:13Z
```

# Add --glob-case-insensitive

---

_@okdana_

Hopefully this is what you had in mind. The flag forces `-g`/`--glob` to be treated as `--iglob`.

If usability on case-insensitive file systems is a concern, i suppose the next logical request would be an option to make the built-in type patterns case-insensitive as well. But i didn't touch that obv

Fixes #1293

---

_@BurntSushi approved on 2019-08-01 21:08_

Awesome, thank you!

---

_Merged by @BurntSushi on 2019-08-01 21:08_

---

_Closed by @BurntSushi on 2019-08-01 21:08_

---
