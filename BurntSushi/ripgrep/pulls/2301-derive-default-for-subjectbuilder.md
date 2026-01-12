```yaml
number: 2301
title: "derive `Default` for `SubjectBuilder`"
type: pull_request
state: closed
author: kraktus
labels: []
assignees: []
base: master
head: patch-1
created_at: 2022-09-06T09:44:53Z
updated_at: 2023-07-08T13:05:53Z
url: https://github.com/BurntSushi/ripgrep/pull/2301
synced_at: 2026-01-12T18:23:14Z
```

# derive `Default` for `SubjectBuilder`

---

_@kraktus_

Hello,

I am reviewing potentially clippy false-positive/false-negative after a change to `clippy::derivable_impls`.
In this case the lint seems correct unless the manual implementation is to emphasise the `strip_dot_prefix: false`. In this case feel free to close

---

_Comment by @BurntSushi on 2023-07-08 13:05_

I think it's just a matter of code evolution. If one adds another boolean that one wants to default to `true`, the `derive` will silently do the wrong thing.

It's not a huge deal. I sometimes derive `Default` for types like this, sometimes not. In this case I think I'd just prefer to leave it as-is. Thanks!

---

_Closed by @BurntSushi on 2023-07-08 13:05_

---
