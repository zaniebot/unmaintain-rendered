---
number: 1339
title: "clap allows required arguments to be omitted if there's a default set"
type: issue
state: closed
author: richardjharris
labels: []
assignees: []
created_at: 2018-09-07T16:06:48Z
updated_at: 2020-02-02T06:17:30Z
url: https://github.com/clap-rs/clap/issues/1339
synced_at: 2026-01-10T01:26:49Z
---

# clap allows required arguments to be omitted if there's a default set

---

_Issue opened by @richardjharris on 2018-09-07 16:06_

### Rust Version

`rustc 1.30.0-nightly (6e0f1cc15 2018-09-05)`

### Affected Version of clap

`"checksum clap 2.32.0 (registry+https://github.com/rust-lang/crates.io-index)" = "b957d88f4b6a63b9d70d5f454ac8011819c6efa7727858f458ab71c756ce2d3e"`

### Bug or Feature Request Summary

I have an arg `-f` which defaults to "1" but can be overriden. Clap allows the user to invoke `myscript -f` without any error (it is set to the default value 1).

```
.arg(Arg::with_name("fields")
            .short("f")
            .long("fields")
            .alias("field")
            .empty_values(false)
            .number_of_values(1)
            .default_value("1")
```

### Expected Behavior Summary

I only want the default of '1' if the option is omitted entirely. If the user specifies `-f` then they are intending to override this default, so if there's nothing after it, it's probably a mistake.

However I can't get clap to do this, even with options like `number_of_values(1)`. The workaround is of course to move the default into `unwrap_or("1")` and manually update the help text.

Not a big deal but if there an option that does want I want, I'd appreciate the tip!

---

_Comment by @mertzt89 on 2018-09-12 21:49_

The phrasing "I only want the default of '1' if the option is omitted entirely" indicates to me that the argument is not intended to be required. Are you wanting the following behavior?

```rust
extern crate clap; // 2.32.0

use clap::{App, Arg};

fn main() {
    let args = App::new("Test Program").arg(
        Arg::with_name("fields")
            .short("f")
            .long("fields")
            .default_value("1")
            .takes_value(true),
    ).get_matches();

    println!("Value is {}", args.value_of("fields").unwrap());
}
```

$ cargo run -- -f 3
Value is 3

$ cargo run
Value is 1

---

_Comment by @richardjharris on 2018-09-12 22:20_

Hi, thank you for the example. That is what I would like except that `cargo run -- -f` should return an error.

My motivation is that (in the UNIX world at least) switches which may or may not take an argument are uncommon and confusing (as they can swallow up other arguments). In my case `-f` is a field number. A user typing `-f` without specifying the field number may have mistaken it for some other flag or simply forgotten to type the number. Typing `--fields` without an argument makes no sense, but I still want to default to `--fields 1` if the switch is missing.

I suspect the answer to this is to use `args.value_of("fields").unwrap_or(1)` and manually add `[default: 1]` to the option's help/long_help. If there is a way to do it that's built into clap though, that would be great :)

---

_Comment by @mertzt89 on 2018-09-12 22:25_

Ah yes, I see the problem now.

With the same code as above:

```
$ cargo run -- -f
Value is 1

$ cargo run -- --fields
Value is 1
```

I agree that I would expect there to be a parsing error especially if `.takes_value(true)` is present.

---

_Comment by @mertzt89 on 2018-09-13 00:19_

Looks like this might be a duplicate of #984 

---

_Referenced in [clap-rs/clap#984](../../clap-rs/clap/issues/984.md) on 2019-11-28 20:38_

---

_Comment by @CreepySkeleton on 2020-02-02 05:23_

Duplicate of #984

---

_Closed by @CreepySkeleton on 2020-02-02 05:23_

---

_Label `T: duplicate` added by @pksunkara on 2020-02-02 06:17_

---
