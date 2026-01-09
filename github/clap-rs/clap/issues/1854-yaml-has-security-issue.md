---
number: 1854
title: Yaml has security issue
type: issue
state: closed
author: yaa110
labels:
  - C-bug
assignees: []
created_at: 2020-04-23T07:35:08Z
updated_at: 2020-04-23T08:03:31Z
url: https://github.com/clap-rs/clap/issues/1854
synced_at: 2026-01-07T13:12:19-06:00
---

# Yaml has security issue

---

_Issue opened by @yaa110 on 2020-04-23 07:35_

```
ID:       RUSTSEC-2018-0006
Crate:    yaml-rust
Version:  0.3.5
Date:     2018-09-17
URL:      https://rustsec.org/advisories/RUSTSEC-2018-0006
Title:    Uncontrolled recursion leads to abort in deserialization
Solution:  upgrade to >= 0.4.1
Dependency tree: 
yaml-rust 0.3.5
└── clap 2.33.0
```

---

_Label `T: bug` added by @yaa110 on 2020-04-23 07:35_

---

_Renamed from "Yaml dependency has security issue" to "Yaml has security issue" by @yaa110 on 2020-04-23 07:35_

---

_Comment by @pksunkara on 2020-04-23 07:51_

Duplicate of #1569 

---

_Closed by @pksunkara on 2020-04-23 07:51_

---

_Label `R: duplicate` added by @pksunkara on 2020-04-23 07:52_

---

_Comment by @CreepySkeleton on 2020-04-23 07:59_

Yes, we [know](https://github.com/clap-rs/clap/issues/1569) about it. But it's not a security issue because
1. `yaml-rust` ever receives only *trusted* input from `clap`. Those `.yml` files are included via `include_str!` at compile time. There's simply no avenue of attack to exploit.
2. As much as we would be willing to simply bump the version, it wouldn't be backward compatible because some types from `yaml-rust` occur in `clap 2.x` public API. The old dependency is `0.3`, the fixed one is `0.4`. Bumping it would break old code for no real benefit (see 1). For the record: this is fixed in `master`, clap 0.3 will not be depending on the vulnerable version.

That said, this is a false positive. @yaa110 would it be possible to mention this comment on `https://rustsec.org/`?

---

_Comment by @yaa110 on 2020-04-23 08:03_

@CreepySkeleton The problem is that the CI (`cargo audit`) is failed due to security issue of `yaml-rust`.

---

_Referenced in [clap-rs/clap#1569](../../clap-rs/clap/issues/1569.md) on 2020-05-05 14:01_

---

_Referenced in [epage/clapng#127](../../epage/clapng/issues/127.md) on 2021-12-06 18:37_

---
