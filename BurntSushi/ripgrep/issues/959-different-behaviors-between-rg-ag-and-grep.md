```yaml
number: 959
title: Different behaviors between rg, ag and grep
type: issue
state: closed
author: hongxuchen
labels:
  - duplicate
assignees: []
created_at: 2018-06-22T07:09:27Z
updated_at: 2018-06-22T10:48:56Z
url: https://github.com/BurntSushi/ripgrep/issues/959
synced_at: 2026-01-12T16:13:22Z
```

# Different behaviors between rg, ag and grep

---

_@hongxuchen_

This is not a big issue however I would like to make it clear.
For now `rg`'s behavior (also `ag`) is different from `grep` when matching text in binary files that contains a bunch of text: by default `rg`/`ag` will not match text when it determines that the file is a binary, but `grep` will still have a try.

As an example, for the file below:
[rg_grep_ag.txt](https://github.com/BurntSushi/ripgrep/files/2126627/rg_grep_ag.txt)
![2018-06-22-150654_1409x172_scrot](https://user-images.githubusercontent.com/843267/41762642-fb9aa46e-762d-11e8-8f72-670e953d8b4d.png)




---

_Comment by @BurntSushi on 2018-06-22 10:48_

Yes, ripgrep specifically skips binary files. This is part of its filtering logic.

Duplicate of #306 and/or #855.

---

_Closed by @BurntSushi on 2018-06-22 10:48_

---

_Label `duplicate` added by @BurntSushi on 2018-06-22 10:48_

---
