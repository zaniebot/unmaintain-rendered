```yaml
number: 1437
title: "Unable to build from source; complaints about missing `unicode_xid` crate"
type: issue
state: closed
author: hibachrach
labels: []
assignees: []
created_at: 2019-12-04T07:55:11Z
updated_at: 2019-12-04T08:25:21Z
url: https://github.com/BurntSushi/ripgrep/issues/1437
synced_at: 2026-01-12T16:13:23Z
```

# Unable to build from source; complaints about missing `unicode_xid` crate

---

_@hibachrach_

#### What version of ripgrep are you using?

I was trying to install 8892bf648cfec111e6e7ddd9f30e932b0371db68 from source.

#### How did you install ripgrep?

I'm attempting to build from source

#### What operating system are you using ripgrep on?

macOS 10.13.6;

```
$ rustc --version                                                                                     
rustc 1.39.0 (4560ea788 2019-11-04)
```

#### Describe your question, feature request, or bug.

When I run `cargo build --release`, I get the following error message:
```
error[E0432]: unresolved import `unicode_xid`
 --> <REDACTED>/.cargo/registry/src/github.com-1ecc6299db9ec823/proc-macro2-1.0.1/src/strnom.rs:5:5
  |
5 | use unicode_xid::UnicodeXID;
  |     ^^^^^^^^^^^ maybe a missing crate `unicode_xid`?

error[E0432]: unresolved import `unicode_xid`
  --> <REDACTED>/.cargo/registry/src/github.com-1ecc6299db9ec823/proc-macro2-1.0.1/src/fallback.rs:15:5
   |
15 | use unicode_xid::UnicodeXID;
   |     ^^^^^^^^^^^ maybe a missing crate `unicode_xid`?

error: aborting due to 2 previous errors

For more information about this error, try `rustc --explain E0432`.
error: Could not compile `proc-macro2`.
```

---

_Renamed from "Unable to build; complaints about missing `unicode_xid` crate" to "Unable to build from source; complaints about missing `unicode_xid` crate" by @hibachrach on 2019-12-04 07:55_

---

_Comment by @hibachrach on 2019-12-04 08:25_

Looks like this was due to my PATH being misconfigured.

---

_Closed by @hibachrach on 2019-12-04 08:25_

---
