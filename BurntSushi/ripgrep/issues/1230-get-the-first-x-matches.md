```yaml
number: 1230
title: Get the first X matches
type: issue
state: closed
author: nitrocode
labels:
  - question
assignees: []
created_at: 2019-03-29T17:15:30Z
updated_at: 2019-03-29T17:29:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1230
synced_at: 2026-01-12T16:13:23Z
```

# Get the first X matches

---

_@nitrocode_

#### What version of ripgrep are you using?

ripgrep 0.10.0

#### How did you install ripgrep?

brew instal ripgrep

#### What operating system are you using ripgrep on?

osx 10.14

#### Describe your question, feature request, or bug.

I'd like to get only the first X matches



---

_Comment by @BurntSushi on 2019-03-29 17:28_

This has been requested before (not sure which issue) and I'm not going to add it. In part because "reporting the first N matches" is an ill-defined operation that may show the same thing each time even if the corpus remains fixed. Instead, you can extract the first 5 matches yourself fairly easily in a deterministic fashion: `rg foo --sort path | head -n5`.

---

_Closed by @BurntSushi on 2019-03-29 17:28_

---

_Label `question` added by @BurntSushi on 2019-03-29 17:28_

---
