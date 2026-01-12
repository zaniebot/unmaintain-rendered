```yaml
number: 1234
title: "ci: switch to xenial"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/xenial
created_at: 2019-04-03T23:28:33Z
updated_at: 2019-04-09T10:48:25Z
url: https://github.com/BurntSushi/ripgrep/pull/1234
synced_at: 2026-01-12T18:23:13Z
```

# ci: switch to xenial

---

_@BurntSushi_

Rust is having problems with trusty, in particular, see this bug I
filed: https://github.com/rust-lang/rust/issues/59411

This was purpotedly fixed in
https://github.com/rust-lang/rust/pull/59468,
but it appears the issue is still occurring.

This commit tries to update to Ubuntu 16.04 in the hope that it will fix
this problem.

---

_Comment by @BurntSushi on 2019-04-03 23:52_

Well... That was easy.

---

_Merged by @BurntSushi on 2019-04-03 23:52_

---

_Closed by @BurntSushi on 2019-04-03 23:52_

---

_Branch deleted on 2019-04-03 23:52_

---

_Comment by @lilianmoraru on 2019-04-09 10:25_

Also, Trusty is EOF in a few days.

---

_Comment by @BurntSushi on 2019-04-09 10:48_

@lilianmoraru Tell that to Travis CI. :-) Trusty is still the default Linux flavor used by them. (I've been very leisurely considering switching to Azure in anticipation of Travis CI going into the shitter.)

---
