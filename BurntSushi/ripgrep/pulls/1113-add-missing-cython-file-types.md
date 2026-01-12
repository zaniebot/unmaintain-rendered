```yaml
number: 1113
title: Add missing Cython file types
type: pull_request
state: merged
author: naufraghi
labels: []
assignees: []
merged: true
base: master
head: add-more-cython-exts
created_at: 2018-11-19T12:11:05Z
updated_at: 2018-11-19T12:37:01Z
url: https://github.com/BurntSushi/ripgrep/pull/1113
synced_at: 2026-01-12T18:23:13Z
```

# Add missing Cython file types

---

_@naufraghi_

From the [Cython file types](https://cython.readthedocs.io/en/latest/src/userguide/language_basics.html?highlight=pxi#cython-file-types) paragraph on the official docs:

>There are three file types in Cython:
>   - The implementation files, carrying a .py or .pyx suffix.
>   - The definition files, carrying a .pxd suffix.
>   - The include files, carrying a .pxi suffix.

So I added `pxi` and `pxd` to the list. (In fact because `ripgrep` was not searching in all my cython files :smile:)

---

_@BurntSushi approved on 2018-11-19 12:21_

Sounds good to me! Thanks!

---

_Merged by @BurntSushi on 2018-11-19 12:37_

---

_Closed by @BurntSushi on 2018-11-19 12:37_

---
