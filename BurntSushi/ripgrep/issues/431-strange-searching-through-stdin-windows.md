```yaml
number: 431
title: Strange searching through stdin. Windows
type: issue
state: closed
author: DoumanAsh
labels: []
assignees: []
created_at: 2017-03-31T13:38:12Z
updated_at: 2017-03-31T13:42:16Z
url: https://github.com/BurntSushi/ripgrep/issues/431
synced_at: 2026-01-12T16:13:22Z
```

# Strange searching through stdin. Windows

---

_@DoumanAsh_

There is strange behavior when printing results of search through stdin:
```
> cat .\src\search_stream.rs | rg "lock\(\)"
    let mut rdr = snap::Reader::new(stdin.lock());
    let mut wtr = stdout.lock();
/baz.rs:10:    let mut rdr = snap::Reader::new(stdin.lock());
/baz.rs-10-    let mut rdr = snap::Reader::new(stdin.lock());
/baz.rs:11:    let mut wtr = stdout.lock();
/baz.rs:10:    let mut rdr = snap::Reader::new(stdin.lock());
/baz.rs-11-    let mut wtr = stdout.lock();
```

It seems rather inconsistent i suppose?


---

_Comment by @BurntSushi on 2017-03-31 13:40_

Why do you think this is an issue with `stdin`? If you search the file directly, do you get the same results?

As far as I can see, the output looks correct. You might be getting confused because of some of the unit tests in `search_stream.rs`. Go do a `^F` for `lock()` in https://github.com/BurntSushi/ripgrep/blob/master/src/search_stream.rs

---

_Comment by @DoumanAsh on 2017-03-31 13:42_

Sorry about that :(

---

_Closed by @DoumanAsh on 2017-03-31 13:42_

---
