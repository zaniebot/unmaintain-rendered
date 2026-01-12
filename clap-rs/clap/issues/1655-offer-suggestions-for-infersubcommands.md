```yaml
number: 1655
title: Offer suggestions for InferSubcommands
type: issue
state: closed
author: CreepySkeleton
labels:
  - C-enhancement
  - A-help
  - E-help-wanted
assignees: []
created_at: 2020-02-02T03:05:59Z
updated_at: 2020-03-01T15:19:34Z
url: https://github.com/clap-rs/clap/issues/1655
synced_at: 2026-01-12T16:14:11Z
```

# Offer suggestions for InferSubcommands

---

_@CreepySkeleton_

Imagine we have this app
```rust
use clap::*;

fn main() {
    let m = clap::App::new("foo")
        .global_setting(AppSettings::InferSubcommands)
        .subcommand(App::new("test"))
        .subcommand(App::new("temp"));

    println!("{:?}", m.get_matches());
}
```

User call it like 
```sh 
$ app te
```

Current error would be 
```
error: The subcommand 'te' wasn't recognized
```
but it would be nice to see something like 
```
app: command 'te' is ambiguous:
    temp test
```

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-02 03:42_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-02 03:43_

---

_Label `D: intermediate` added by @CreepySkeleton on 2020-02-02 03:43_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-02 03:43_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-02 03:43_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-02 03:43_

---

_Comment by @pickfire on 2020-02-29 04:45_

I would like to give this a try. I think `suggestions::did_you_mean` now needs to be:
```rust
pub fn did_you_mean<T, I>(v: &str, possible_values: I) -> Vec<String>
where
    T: AsRef<str>,
    I: IntoIterator<Item = T>,
{
```
from
```rust
pub fn did_you_mean<T, I>(v: &str, possible_values: I) -> Option<String>
where
    T: AsRef<str>,
    I: IntoIterator<Item = T>,
{
```

---

_Comment by @CreepySkeleton on 2020-02-29 05:46_

@pickfire Go ahead! You can tweak any API as long as it's internal (the function in question is) without justification.

---

_Comment by @pickfire on 2020-03-01 08:29_

@CreepySkeleton Would it be better to be?
```
        Did you mean 'test' or 'temp'?
```
Right now, I see that it have 
```rust
#[cfg(feature = "suggestions")]
static DYM_SUBCMD: &str = "error: The subcommand 'subcm' wasn't recognized
        Did you mean 'subcmd'?

If you believe you received this message in error, try re-running with 'dym -- subcm'

USAGE:
    dym [SUBCOMMAND]

For more information try --help";
```

---

_Comment by @pksunkara on 2020-03-01 08:30_

Yup, let's put all the allowed completions.

---

_Referenced in [clap-rs/clap#1710](../../clap-rs/clap/pulls/1710.md) on 2020-03-01 09:09_

---

_Closed by @bors[bot] on 2020-03-01 15:19_

---

_Closed by @bors[bot] on 2020-03-01 15:19_

---
