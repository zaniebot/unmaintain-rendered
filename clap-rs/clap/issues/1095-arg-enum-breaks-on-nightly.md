```yaml
number: 1095
title: "`arg_enum` breaks on nightly"
type: issue
state: closed
author: epage
labels:
  - C-bug
assignees: []
created_at: 2017-11-06T15:46:01Z
updated_at: 2021-05-26T11:10:51Z
url: https://github.com/clap-rs/clap/issues/1095
synced_at: 2026-01-12T16:14:10Z
```

# `arg_enum` breaks on nightly

---

_@epage_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.23.0-nightly (3b82e4c74 2017-11-05)

### Affected Version of clap

* 2.27.1

```
error: unused import: `std::ascii::AsciiExt`
 --> src/syntax_highlight/syntect.rs:1:5
  |
1 | use std::ascii::AsciiExt;
  |     ^^^^^^^^^^^^^^^^^^^^
  |
note: lint level defined here
 --> src/lib.rs:35:9
  |
35|         unused_imports,
  |         ^^^^^^^^^^^^^^
error: unused import: `:: std :: ascii :: AsciiExt`
  --> src/config.rs:15:1
   |
15 | / arg_enum! {
16 | |     #[derive(Serialize, Deserialize)]
17 | |     #[derive(Debug, PartialEq, Copy, Clone)]
18 | |     pub enum Dump {
...  |
23 | |     }
24 | | }
   | |_^
   |
   = note: this error originates in a macro outside of the current crate
```

See https://travis-ci.org/cobalt-org/cobalt.rs/jobs/298020332

---

_Comment by @epage on 2017-11-06 17:41_

See https://users.rust-lang.org/t/psa-dealing-with-warning-unused-import-std-ascii-asciiext-in-today-s-nightly/13726

---

_Comment by @kbknapp on 2017-11-06 17:56_

Ah great catch! This should be an easy fix once I get home. Adding a `cfg` to either remove the import or simply allow that lint on nightly is fine with me.

---

_Label `C: macros` added by @kbknapp on 2017-11-06 17:57_

---

_Label `P3: want to have` added by @kbknapp on 2017-11-06 17:57_

---

_Label `T: nightly bug` added by @kbknapp on 2017-11-06 17:57_

---

_Label `W: 2.x` added by @kbknapp on 2017-11-06 17:57_

---

_Added to milestone `2.27.2` by @kbknapp on 2017-11-06 17:57_

---

_Comment by @kbknapp on 2017-11-07 01:18_

As of #1096 you should compile clap with the `nightly` feature which fixes this issue. See the [commit f858aa2 message](https://github.com/kbknapp/clap-rs/commit/fb08bb5dd5760d2932e9f673abe23ea0df858aa2) for all the details.

I remember a while back some discussion lints not being transitive to your dependencies, but I don't know where that discussion went. I've adopted the approach of only denying warnings in my "strict" or "lint" feature that I only use while developing or in CI, but it won't block a PR merge.

---

_Comment by @epage on 2017-11-07 01:57_

Thanks for being quick about this.

The downside to this approach is it requires clients to create a `nightly` feature so that we can successful build our strict CIs.  Is there a reason you didn't take the approach of disabling the warning for just that one `use`?

---

_Comment by @kbknapp on 2017-11-07 02:39_

> Is there a reason you didn't take the approach of disabling the warning for just that one use?

Because on stable that `use` is still required, and there isn't a built-in `cfg` to determine when you're building on stable/nightly unless you manually make a feature for it (that I'm aware of).

If there's a better way I'm open to ideas :wink: 

---

_Closed by @kbknapp on 2017-11-07 02:48_

---

_Comment by @kbknapp on 2017-11-07 02:49_

This issue was closed automatically via the PR, but if there is a better way to handle this issue I'm open to ideas and willing to consider them :+1: 

---

_Comment by @epage on 2017-11-07 03:37_

In the linked post on users, it suggests
```rust
#[allow(unused_import)] use std::ascii::AsciiExt;
```

---

_Comment by @kbknapp on 2017-11-07 04:42_

Yep just saw that, I think I only saw your CI link in my haste. I'll try this out tomorrow and put in the new PR, thanks for pointing it out :+1:

---

_Reopened by @kbknapp on 2017-11-07 04:42_

---

_Referenced in [clap-rs/clap#1102](../../clap-rs/clap/pulls/1102.md) on 2017-11-12 17:51_

---

_Closed by @kbknapp on 2017-11-12 19:06_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-12 19:13_

---

_Label `T: bug` added by @pksunkara on 2021-05-26 11:10_

---

_Label `T: nightly bug` removed by @pksunkara on 2021-05-26 11:10_

---
