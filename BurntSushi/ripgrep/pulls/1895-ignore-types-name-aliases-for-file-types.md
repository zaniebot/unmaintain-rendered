```yaml
number: 1895
title: "ignore/types: name aliases for file types"
type: pull_request
state: closed
author: kotborealis
labels:
  - rollup
assignees: []
base: master
head: i-1857
created_at: 2021-06-15T14:03:57Z
updated_at: 2023-07-08T22:53:08Z
url: https://github.com/BurntSushi/ripgrep/pull/1895
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: name aliases for file types

---

_@kotborealis_

Closes #1857

* Changed `ignore/types/default_types.rs` to allow array of names, which allows aliases — like `py==python` — without specifying globs twice.
* Updated `ignore/types/types.rs` to use name aliases
* Updated tests in `ignore/types/types.rs`


---

_@matkoniecz approved on 2022-03-13 08:01_

note: I lack Rust knowledge and have not run this code. But from looking at diff it looks good and wanted to add thank for implementing this.

Having `ruby` type and `py` type without aliases is very confusing for someone new.

---

_Review requested from @matkoniecz by @kotborealis on 2022-10-10 19:40_

---

_Comment by @kotborealis on 2022-10-10 19:52_

Well, almost year and a half later I've managed to remember about this PR.
I have rebased my work, solved conflicts and tested it once more. Should we finally merge this one? 😅 

---

_Label `rollup` added by @BurntSushi on 2023-07-08 17:57_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
