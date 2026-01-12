```yaml
number: 339
title: Implement Hash for Glob
type: pull_request
state: merged
author: stuhood
labels: []
assignees: []
merged: true
base: master
head: stuhood/hash-for-glob
created_at: 2017-01-23T02:17:42Z
updated_at: 2017-02-18T16:46:09Z
url: https://github.com/BurntSushi/ripgrep/pull/339
synced_at: 2026-01-12T18:23:12Z
```

# Implement Hash for Glob

---

_@stuhood_

Implement `Hash` for `Glob`, and re-implement `PartialEq` using only non-redundant struct fields.

I've been using the globset crate as a library (thank you!), and ran across the fact that `Glob` was un-hashable while deduping user inputs.

---

_Merged by @BurntSushi on 2017-02-18 16:46_

---

_Closed by @BurntSushi on 2017-02-18 16:46_

---

_Comment by @BurntSushi on 2017-02-18 16:46_

Thanks!

---
