```yaml
number: 1007
title: add Idris, Dhall, and ATS
type: pull_request
state: merged
author: vmchale
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2018-08-07T16:47:10Z
updated_at: 2018-08-07T17:10:20Z
url: https://github.com/BurntSushi/ripgrep/pull/1007
synced_at: 2026-01-12T18:23:13Z
```

# add Idris, Dhall, and ATS

---

_@vmchale_

This adds support for Idris, Dhall, and ATS to the `ignore` crate.

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:111 on 2018-08-07 16:51_

I don't think I've seen `cats` used for a C file before. Where does that come from?

---

_@BurntSushi reviewed on 2018-08-07 16:52_

Nice! Looks good, with one question/clarification.

---

_@vmchale reviewed on 2018-08-07 16:53_

---

_Review comment by @vmchale on `ignore/src/types.rs`:111 on 2018-08-07 16:53_

It is used with ATS to contain C code called by ATS code later, e.g. https://groups.google.com/forum/#!topic/ats-lang-users/On6L-1e6p_0

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:152 on 2018-08-07 16:54_

Same question as for `cats` I think. When and where are these file extensions used?

---

_@BurntSushi reviewed on 2018-08-07 16:54_

---

_@BurntSushi reviewed on 2018-08-07 16:56_

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:111 on 2018-08-07 16:56_

Great. Thank you.

---

_@vmchale reviewed on 2018-08-07 16:58_

---

_Review comment by @vmchale on `ignore/src/types.rs`:152 on 2018-08-07 16:58_

`.hsc` is for the `hsc2hs` preprocessor. You can see an example here: https://github.com/haskell-foundation/foundation/blob/master/foundation/Foundation/Foreign/MemoryMap/Posix.hsc

`.cpphs` is just for the `cpphs` preprocessor, e.g. here: https://github.com/vmchale/atspkg/blob/master/ats-pkg/internal/Quaalude.cpphs

`c2hs` is another pre-processor, and there is a nice example here: https://github.com/haskell/c2hs/blob/master/examples/libghttpHS/Ghttp.chs

---

_@BurntSushi reviewed on 2018-08-07 17:02_

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:152 on 2018-08-07 17:02_

All righty, I buy it, thanks for showing me those!

---

_@BurntSushi approved on 2018-08-07 17:03_

---

_@vmchale reviewed on 2018-08-07 17:05_

---

_Review comment by @vmchale on `ignore/src/types.rs`:152 on 2018-08-07 17:05_

No problem! 

---

_Merged by @BurntSushi on 2018-08-07 17:10_

---

_Closed by @BurntSushi on 2018-08-07 17:10_

---
