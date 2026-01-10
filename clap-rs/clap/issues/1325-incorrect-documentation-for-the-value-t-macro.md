---
number: 1325
title: Incorrect documentation for the value_t macro
type: issue
state: closed
author: kgnlp
labels:
  - A-docs
assignees: []
created_at: 2018-07-23T11:12:12Z
updated_at: 2020-04-04T15:50:35Z
url: https://github.com/clap-rs/clap/issues/1325
synced_at: 2026-01-10T01:26:48Z
---

# Incorrect documentation for the value_t macro

---

_Issue opened by @kgnlp on 2018-07-23 11:12_

The documentation for value_t (https://github.com/kbknapp/clap-rs/blob/master/src/macros.rs#L35) says the macro returns a `Result<T,String>` while it actually returns a `Result<T, clap::Error>` aka `clap::Result<T>`

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-02-02 07:35_

---

_Label `C: docs` added by @CreepySkeleton on 2020-02-02 07:35_

---

_Added to milestone `3.0.0-alpha.3` by @CreepySkeleton on 2020-02-02 07:35_

---

_Unassigned @CreepySkeleton by @CreepySkeleton on 2020-02-02 07:36_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-02 07:36_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-02 07:36_

---

_Removed from milestone `3.0.0-alpha.3` by @CreepySkeleton on 2020-02-02 07:36_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-02 07:36_

---

_Comment by @Dylan-DPC-zz on 2020-04-02 19:18_

hi @AimainaHito thanks for the issue. are you open to sending a PR that changes the behaviour ? (easy fix)

---

_Referenced in [clap-rs/clap#1779](../../clap-rs/clap/pulls/1779.md) on 2020-04-03 18:36_

---

_Closed by @bors[bot] on 2020-04-04 15:50_

---
