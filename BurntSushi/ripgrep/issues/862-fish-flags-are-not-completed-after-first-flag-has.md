```yaml
number: 862
title: "[fish] Flags are not completed after first flag has been completed"
type: issue
state: closed
author: zx8
labels:
  - bug
assignees: []
created_at: 2018-03-19T18:50:34Z
updated_at: 2018-04-19T17:35:05Z
url: https://github.com/BurntSushi/ripgrep/issues/862
synced_at: 2026-01-12T16:13:22Z
```

# [fish] Flags are not completed after first flag has been completed

---

_@zx8_

#### What version of ripgrep are you using?

`ripgrep 0.8.1`

#### Expected Behavior Summary
```
$ rg --<TAB>
# list of completions
$ rg --no-follow --<TAB>
# list of completions
```

#### Actual Behavior Summary
```
$ rg --<TAB>
# works - list of completions
$ rg --no-follow --<TAB>
# broken - no completions
```
---

Cross-post of https://github.com/kbknapp/clap-rs/issues/1212 – fixed in clap v2.31.2.

Would it be possible to bump ripgrep's clap dependency to pick up the fix (or manually generate the fish completions again using the fixed version of clap)?

---

_Comment by @BurntSushi on 2018-04-19 17:34_

This was fixed in https://github.com/BurntSushi/ripgrep/commit/34abed597fd119fbe6ca4fe0a820808d980f46c9

---

_Closed by @BurntSushi on 2018-04-19 17:35_

---

_Label `bug` added by @BurntSushi on 2018-04-19 17:35_

---
