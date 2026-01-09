---
number: 3410
title: "Short options with arguments conflicts with `allow_hyphen_values`"
type: issue
state: open
author: 197g
labels:
  - C-enhancement
  - A-parsing
  - E-medium
assignees: []
created_at: 2022-02-06T17:06:19Z
updated_at: 2025-05-27T17:02:27Z
url: https://github.com/clap-rs/clap/issues/3410
synced_at: 2026-01-07T13:12:19-06:00
---

# Short options with arguments conflicts with `allow_hyphen_values`

---

_Issue opened by @197g on 2022-02-06 17:06_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.0

### Describe your use case

The program GNU `seq` requires us to parse floating point values as arguments, which may be negative or have formatting different from the standard `<f64 as FromStr>` style. At the same time it accepts some short options to control the output. The problematic case is combining those two: we must utilize `AllowHyphenValues` while also accepting short option values.

```bash
$ seq -s, -1 2
-1,0,1,2
$ # Demonstrating non-f64-style arguments that must also match positional values:
$ seq -0x.ep-3 -0x.1p-3 -0x.fp-3
-0,109375
-0,117188
```

This is solved in GNU by external iteration over the arguments where `seq` itself decides what constitutes a flag, and what starts the value arguments: <https://github.com/coreutils/coreutils/blob/master/src/seq.c#L593-L604>

In clap, however, when `AllowHyphenValues` is active then only short options character are allowed to appear in an option. Otherwise, it is interpreted as a position argument. See https://github.com/clap-rs/clap/blob/5c3868ea4cb8063731d8526e8e97414942a987ae/src/parse/parser.rs#L994-L995



### Describe the solution you'd like

Now, I would propose to add a new setting that slightly modifies these rules. Where an hyphenated argument may _start_ with short options but where short option parsing stops at the first value-taking option, and any other characters are permitted when such an option is recognized. That is, in this context, where the short options `w` and `s` exist, `-ws,` would be interpreted as:

1. `w` matches a short option without argument, continue.
2. `s` matches a short option that does take an argument, switching to value mode because this option takes a value.
3. `-` is ignored as part of the value.
4. The argument `-ws,` is interpreted as options.


### Alternatives, if applicable

A more intricate solution would be to enable external iteration, where arguments are consumed by clap one-by-one. That is, more like the GNU style parsing loop where some `get_first_argument(iterator)` consumes only some arguments from the iterator but returns control to the caller after the first option has been consumed. This could enable an iteration loop in the style of GNU `seq` where the arguments are pre-tested by the program logic on whether they constitute a positional argument; allowing us to break manually instead of require `AllowHyphenValues` to control `clap` to do this internally.

### Additional Context



---

_Label `C-enhancement` added by @197g on 2022-02-06 17:06_

---

_Comment by @197g on 2022-02-06 17:07_

Sorry, I forgot to fill out additional context: It's mentioned in `uutils/coreutils`: <https://github.com/uutils/coreutils/pull/3081>


---

_Label `A-parsing` added by @epage on 2022-02-07 15:09_

---

_Label `E-medium` added by @epage on 2022-02-07 15:09_

---

_Comment by @epage on 2022-02-07 15:10_

> Now, I would propose to add a new setting that slightly modifies these rules. Where an hyphenated argument may start with short options but where short option parsing stops at the first value-taking option, and any other characters are permitted when such an option is recognized

I think this makes sense

> A more intricate solution would be to enable external iteration, where arguments are consumed by clap one-by-one. That is, more like the GNU style parsing loop where some get_first_argument(iterator) consumes only some arguments from the iterator but returns control to the caller after the first option has been consumed. This could enable an iteration loop in the style of GNU seq where the arguments are pre-tested by the program logic on whether they constitute a positional argument; allowing us to break manually instead of require AllowHyphenValues to control clap to do this internally.

This sounds more like `lexopt`.  We have plans to modularize clap so we provide a lexopt-like crate and allow you to use all of the other parts of clap built up together.

---

_Referenced in [clap-rs/clap#3450](../../clap-rs/clap/issues/3450.md) on 2022-02-11 22:06_

---

_Referenced in [getsentry/sentry-cli#1186](../../getsentry/sentry-cli/issues/1186.md) on 2022-04-12 10:12_

---

_Referenced in [clap-rs/clap#6018](../../clap-rs/clap/issues/6018.md) on 2025-05-27 17:00_

---

_Renamed from "Short options with arguments conflicts with AllowHyphenValues" to "Short options with arguments conflicts with `allow_hyphen_values`" by @epage on 2025-05-27 17:00_

---

_Comment by @epage on 2025-05-27 17:01_

If I'm reading this correct, this is the same issue as #6018.  A more modern reproduction case is
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["debug"] }
---

use std::ffi::OsStr;

use clap::Command;
use clap::arg;


fn main() {
    let mut cmd = Command::new("build")
        .arg(arg!(-P --parallel <N>))
        .arg(arg!(<ARGS>...).trailing_var_arg(true).allow_hyphen_values(true));
    cmd.build();
    dbg!(&cmd);

    let matches = if false {
        cmd.get_matches_from(vec!["build", "-P", "32", "foo", "--bar"])
    } else {
        cmd.get_matches_from(vec!["build", "-P32", "foo", "--bar"])
    };

    dbg!(&matches);

    assert_eq!(matches.get_one("parallel"), Some(&String::from("32")));
    let mut args = matches.get_raw("ARGS").unwrap().into_iter();
    assert_eq!(args.next(), Some(OsStr::new("foo")));
    assert_eq!(args.next(), Some(OsStr::new("--bar")));
}
```
(`if true` will succeed, `if false` will panic).


---

_Comment by @epage on 2025-05-27 17:02_

What I'm wondering is why we didn't consider just changing the behavior so that a short that takes a value is always considered a short, rather than a `MaybeHyphenValue`.

---
