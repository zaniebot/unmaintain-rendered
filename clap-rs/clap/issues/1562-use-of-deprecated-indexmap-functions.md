---
number: 1562
title: Use of deprecated indexmap functions 
type: issue
state: closed
author: Dylan-DPC-zz
labels:
  - E-easy
assignees: []
created_at: 2019-10-04T12:36:08Z
updated_at: 2019-10-21T10:26:41Z
url: https://github.com/clap-rs/clap/issues/1562
synced_at: 2026-01-10T01:26:57Z
---

# Use of deprecated indexmap functions 

---

_Issue opened by @Dylan-DPC-zz on 2019-10-04 12:36_

If you add `#[deprecated]` to lib.rs on master (3.0.0 alpha-1 candidate) we get the following warnings: 

```
warning: use of deprecated item 'indexmap::IndexMap::<K, V, S>::remove': use `swap_remove`
  --> src/parse/arg_matcher.rs:80:53
   |
80 |     pub fn remove(&mut self, arg: Id) { self.0.args.remove(&arg); }
   |                                                     ^^^^^^
   |
   = note: #[warn(deprecated)] on by default

warning: use of deprecated item 'indexmap::IndexMap::<K, V, S>::remove': use `swap_remove`
  --> src/parse/arg_matcher.rs:85:25
   |
85 |             self.0.args.remove(arg);
   |                         ^^^^^^
```

This should be an easy fix, so if anyone wants to take this as their first contribution to clap, go ahead and submit a PR :)

---

_Label `good first issue` added by @Dylan-DPC-zz on 2019-10-04 14:02_

---

_Comment by @alarsyo on 2019-10-04 21:30_

Hi, I'll try to fix this one!

---

_Referenced in [clap-rs/clap#1563](../../clap-rs/clap/pulls/1563.md) on 2019-10-04 22:09_

---

_Closed by @Dylan-DPC-zz on 2019-10-21 10:26_

---
