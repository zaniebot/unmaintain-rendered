```yaml
number: 1089
title: Panic when searching binary file as utf-16le
type: issue
state: closed
author: comex
labels:
  - bug
assignees: []
created_at: 2018-10-22T00:22:57Z
updated_at: 2018-10-22T10:52:41Z
url: https://github.com/BurntSushi/ripgrep/issues/1089
synced_at: 2026-01-12T16:13:22Z
```

# Panic when searching binary file as utf-16le

---

_@comex_

Seems similar to #1052, but I'm encountering this on current ripgrep trunk (acf226c39d).

With the following file: https://a.qoid.us/System.Collections.dll

Running `rg -E utf-16le a System.Collections.dll` produces:
```
thread 'main' panicked at 'index out of bounds: the len is 2 but the index is 2', /Users/comex/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs-0.8.6/src/handles.rs:311:21
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

---

_Closed by @BurntSushi on 2018-10-22 10:51_

---

_Comment by @BurntSushi on 2018-10-22 10:52_

Thanks! This was indeed occurring on master. It looks like bumping to the latest release of `encoding_rs` fixes this.

---

_Label `bug` added by @BurntSushi on 2018-10-22 10:52_

---
