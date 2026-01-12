```yaml
number: 506
title: Using an unparenthesized pipe with -w produces unexpected results
type: issue
state: closed
author: scottmcm
labels:
  - bug
  - help wanted
assignees: []
created_at: 2017-06-07T02:52:45Z
updated_at: 2017-06-15T10:55:56Z
url: https://github.com/BurntSushi/ripgrep/issues/506
synced_at: 2026-01-12T16:13:22Z
```

# Using an unparenthesized pipe with -w produces unexpected results

---

_@scottmcm_

I ran `rg -w 'min|max' src/lib*` in the rust source, and got far more results than I expected.  For example:
```
src/liblibc\src\unix\solaris\mod.rs
17:pub type minor_t = ::c_uint;
250:        pub f_namemax: ::c_ulong,
1070:    pub fn mincore(addr: *const ::c_void, len: ::size_t,
```

It seems to be matching `(\bmin)|(max\b)`, whereas I expected `\b(min|max)\b`.

repro'd in 0.4.0 and 0.5.2


---

_Comment by @BurntSushi on 2017-06-07 03:10_

Hah. Nice. Looks like you are right: https://github.com/BurntSushi/ripgrep/blob/master/src/args.rs#L508

---

_Label `bug` added by @BurntSushi on 2017-06-08 10:55_

---

_Label `help wanted` added by @BurntSushi on 2017-06-08 10:55_

---

_Comment by @BurntSushi on 2017-06-08 10:57_

This should be a relatively easy bug if anyone wants to take a crack at it. Basically, all that needs to be done is to put a non-capturing group around the inner expression here: https://github.com/BurntSushi/ripgrep/blob/13235b596f93f7f0e7ce76e7967996787147e96e/src/args.rs#L508 --- And put a regression test in `tests/tests.rs` (search for `regression` to see examples of other regression tests).

---

_Closed by @BurntSushi on 2017-06-15 10:55_

---
