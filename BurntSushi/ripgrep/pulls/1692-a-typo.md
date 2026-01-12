```yaml
number: 1692
title: A typo?
type: pull_request
state: closed
author: qm3ster
labels: []
assignees: []
base: master
head: patch-1
created_at: 2020-09-29T08:27:33Z
updated_at: 2020-09-29T13:52:25Z
url: https://github.com/BurntSushi/ripgrep/pull/1692
synced_at: 2026-01-12T18:23:14Z
```

# A typo?

---

_@qm3ster_

A cursory glance at TYPO3 documentation suggests they use `.typoscript` files.
however, there is `"*.vue"` in the `"js"` category, perhaps `.ts` should go there as well?

---

_Comment by @BurntSushi on 2020-09-29 13:08_

TypoScript was added in https://github.com/BurntSushi/ripgrep/pull/1477, and as far as I can tell, it's correct.

Typescript already has a type in ripgrep, but it's called `ts`.

---

_Comment by @qm3ster on 2020-09-29 13:52_

Ah, I see it now. And having multiple types claim an extension should be fine too, there are three instances of `"*.v"` for example.
Sorry.

---

_Closed by @qm3ster on 2020-09-29 13:52_

---

_Branch deleted on 2020-09-29 13:52_

---
