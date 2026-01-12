```yaml
number: 1345
title: minor update to manpage to reflect glob matching behavior change
type: pull_request
state: merged
author: LawAbidingCactus
labels: []
assignees: []
merged: true
base: master
head: documentation-update
created_at: 2019-08-07T16:59:42Z
updated_at: 2019-08-07T17:53:07Z
url: https://github.com/BurntSushi/ripgrep/pull/1345
synced_at: 2026-01-12T18:23:13Z
```

# minor update to manpage to reflect glob matching behavior change

---

_@LawAbidingCactus_

This is a minor update to ripgrep's manpage to reflect the changes in #762. The manpage's use of `!git/*` in glob patten matching examples, which is presumably intended to exclude all `git` directories from ripgrep's search, is incorrect. This might be a source of confusion for people configuring ripgrep who may not be familiar with `.gitignore` semantics.

---

_Review comment by @BurntSushi on `doc/rg.1.txt.tpl`:146 on 2019-08-07 17:06_

Ah interesting, yeah, this makes sense to me. We might as well go the whole way here and use `.git` instead of `git`. What do you think?

---

_@BurntSushi reviewed on 2019-08-07 17:07_

---

_@LawAbidingCactus reviewed on 2019-08-07 17:08_

---

_Review comment by @LawAbidingCactus on `doc/rg.1.txt.tpl`:146 on 2019-08-07 17:08_

Seems fine! That's what I usually use to exclude `.git` dirs anyway.

---

_@BurntSushi approved on 2019-08-07 17:19_

LGTM.

---

_Merged by @BurntSushi on 2019-08-07 17:47_

---

_Closed by @BurntSushi on 2019-08-07 17:47_

---

_Branch deleted on 2019-08-07 17:53_

---
