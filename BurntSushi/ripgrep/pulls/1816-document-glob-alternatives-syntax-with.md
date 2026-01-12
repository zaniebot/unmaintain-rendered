```yaml
number: 1816
title: "Document glob alternatives syntax with {}"
type: pull_request
state: closed
author: ilyagr
labels:
  - rollup
assignees: []
base: master
head: patch-1
created_at: 2021-03-09T22:13:04Z
updated_at: 2021-06-01T01:51:21Z
url: https://github.com/BurntSushi/ripgrep/pull/1816
synced_at: 2026-01-12T18:23:14Z
```

# Document glob alternatives syntax with {}

---

_@ilyagr_

This syntax does not exist in `git`, so it is not documented in `man gitignore`. There is
a question of whether it *should* exist, but as long as it does, it should be documented
somewhere.

See also:
https://github.com/BurntSushi/ripgrep/issues/1221
https://github.com/BurntSushi/ripgrep/issues/1368

-------

**Update 1:** I'm especially interested in documenting the lack of empty alternatives, since the reason I
started looking for documentation is that I couldn't figure out whether `-g .git{,/*}` *should*
work.

**Update 2:** I like this syntax, but I'd prefer it if it was only valid in the command line (as opposed to ignore files), FWIW. That is, as long as having a slightly different syntax in the command line doesn't introduce too much complexity.

---

_@BurntSushi approved on 2021-05-30 13:28_

---

_Label `rollup` added by @BurntSushi on 2021-05-30 13:29_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
