---
number: 1762
title: Short aliases use numbers, expand macro fails
type: issue
state: closed
author: Stzx
labels:
  - C-bug
assignees: []
created_at: 2020-03-26T08:16:11Z
updated_at: 2020-04-12T22:28:26Z
url: https://github.com/clap-rs/clap/issues/1762
synced_at: 2026-01-10T01:27:05Z
---

# Short aliases use numbers, expand macro fails

---

_Issue opened by @Stzx on 2020-03-26 08:16_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.44.0-nightly (f509b26a7 2020-03-18)`

### Code

```rust
use clap::clap_app;

fn main() {
    clap_app!( MyApp =>
        (@arg IPV6: -6 "IPv6")
    );
}
```
### Steps to reproduce the issue

`cargo check`

### Affected Version of `clap*`

`clap = "2.33.0"`

### Actual Behavior Summary

Failed to compile.

```
4  | /     clap_app!( MyApp =>
5  | |         (@arg IPV6: -6 "IPv6")
6  | |     );
   | |______^ no rules expected this token in macro call
```

### Expected Behavior Summary

Compile successfully.

### Additional context
Add any other context about the problem here.


---

_Label `T: bug` added by @Stzx on 2020-03-26 08:16_

---

_Comment by @CreepySkeleton on 2020-03-26 08:49_

That is true, `clap_app!` does not support number literals here, it must be ident, like `--foo` or `--v6`.

AFAIK there's no way to do it with `clap_app!`, you have to explicitly build the app instead:
```rust
App::new("MyApp")
    .arg(Arg::with_name("IPV6").short("6"))
```

---

_Comment by @pksunkara on 2020-04-01 21:43_

I am not even sure this is possible because `macro_rules` can only do `expr` and not `lit` or `number`.

---

_Comment by @Stzx on 2020-04-02 01:22_

Let us tell users this in examples and documentation ?

---

_Comment by @CreepySkeleton on 2020-04-07 13:53_

In fact, this there's a way to use string literals for long options:
```rust
use clap::{App, Arg, clap_app};

fn main() {
    let matches = clap_app!( MyApp =>
        (@arg IPV6: --("6") "IPv6")
    );
}
```

But `-("...")` for short options is not supported. @pksunkara , what do you think about it?

---

_Comment by @pksunkara on 2020-04-07 13:58_

Because we did not support `expr` for `short` in our macro definition.

Something similar to this line, https://github.com/clap-rs/clap/blob/master/src/macros.rs#L755, should be enough. But, as I said, I am leery about supporting `expr` like this.

---

_Comment by @CreepySkeleton on 2020-04-07 14:04_

We already have support for long options, just not for short ones which feels inconsistent.

---

_Label `C: macros` added by @pksunkara on 2020-04-09 08:31_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-12 08:47_

---

_Referenced in [clap-rs/clap#1813](../../clap-rs/clap/pulls/1813.md) on 2020-04-12 10:03_

---

_Closed by @bors[bot] on 2020-04-12 22:28_

---
