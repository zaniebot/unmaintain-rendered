```yaml
number: 2074
title: Support omitting the lowest number of repetitions
type: issue
state: closed
author: ghost
labels:
  - wontfix
assignees: []
created_at: 2021-11-17T03:38:08Z
updated_at: 2021-11-17T14:08:20Z
url: https://github.com/BurntSushi/ripgrep/issues/2074
synced_at: 2026-01-12T16:13:24Z
```

# Support omitting the lowest number of repetitions

---

_@ghost_

grep supports expressions like `.{,10}` (which is the same as `.{0,10}`), but when I try it with ripgrep, I get this error:

`error: repetition quantifier expects a valid decimal`

---

_Comment by @BurntSushi on 2021-11-17 14:07_

This is intended behavior and is documented: https://docs.rs/regex/1.5.4/regex/#repetitions

---

_Closed by @BurntSushi on 2021-11-17 14:07_

---

_Label `wontfix` added by @BurntSushi on 2021-11-17 14:07_

---
