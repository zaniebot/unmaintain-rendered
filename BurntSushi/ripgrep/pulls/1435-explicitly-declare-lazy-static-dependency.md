```yaml
number: 1435
title: explicitly declare lazy_static dependency
type: pull_request
state: closed
author: infinity0
labels:
  - rollup
assignees: []
base: master
head: patch-2
created_at: 2019-11-25T10:04:36Z
updated_at: 2020-02-17T22:16:32Z
url: https://github.com/BurntSushi/ripgrep/pull/1435
synced_at: 2026-01-12T18:23:13Z
```

# explicitly declare lazy_static dependency

---

_@infinity0_

`benches/bench.rs` uses lazy_static but Cargo.toml does not declare a dependency on it. This causes rustc to use its own internal private copy instead. Sometimes this causes unintuitive errors like Debian bug [942243](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=942243).

The underlying issue is rust-lang/rust#27812 but it can be avoided by explicitly declaring the dependency, which you are supposed to do anyways.

---

_Label `rollup` added by @BurntSushi on 2020-02-15 15:18_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
