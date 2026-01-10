---
number: 4351
title: "Can't use ValueParser to replace PossibleValues in v4"
type: issue
state: closed
author: o2sh
labels:
  - C-bug
  - A-docs
assignees: []
created_at: 2022-10-05T22:51:18Z
updated_at: 2022-10-26T16:15:11Z
url: https://github.com/clap-rs/clap/issues/4351
synced_at: 2026-01-10T01:27:53Z
---

# Can't use ValueParser to replace PossibleValues in v4

---

_Issue opened by @o2sh on 2022-10-05 22:51_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.64.0

### Clap Version

4.08

### Minimal reproducible code

```rust
static COLOR_RESOLUTIONS: [&str; 5] = ["16", "32", "64", "128", "256"];

#[arg(
    long,
    value_name = "VALUE",
    requires = "image",
    default_value_t = 16usize,
    // possible_values = ["16", "32", "64", "128", "256"], // Used to work before v4
    value_parser = PossibleValuesParser::new(COLOR_RESOLUTIONS) // fails
)]
pub color_resolution: usize,
```

```sh
$ cargo run

thread 'main' panicked at 'Mismatch between definition and access of `color_resolution`. Could not downcast to TypeId { t: 13834754221672687376 }, need to downcast to TypeId { t: 5005868189860995316 }

```


---

_Label `C-bug` added by @o2sh on 2022-10-05 22:51_

---

_Referenced in [clap-rs/clap#4352](../../clap-rs/clap/pulls/4352.md) on 2022-10-05 23:57_

---

_Label `A-docs` added by @epage on 2022-10-05 23:57_

---

_Comment by @epage on 2022-10-05 23:59_

For now, `PossibleValuesParser` only works with `String` though you can use `TypedValueParser::map` to adapt the type.  In #4352, I'm adding some examples on how to do this.

---

_Closed by @epage on 2022-10-06 01:35_

---

_Comment by @aldanor on 2022-10-26 15:25_

> For now, PossibleValuesParser only works with String

Just wondering, is it planned to have it back working with other types eventually? (e.g. that implement FromStr?)

Chances are, these lines will be copied/pasted many-many times all over the place (it's just one of the most common use cases for the old `possible_values`):

```rust
use clap::builder::{PossibleValuesParser, TypedValueParser as _};
...
    value_parser = PossibleValuesParser::new(Foo::VARIANTS)
        .map(|s| s.parse::<Foo>().unwrap())
```

E.g.:

```rust
    value_parser = PossibleValuesFromStr::<Foo>::new(Foo::VARIANTS)
```

---

_Comment by @epage on 2022-10-26 15:30_

Feel free to create an issue for making it more convenient.

---

_Comment by @aldanor on 2022-10-26 15:58_

@epage An issue or a PR? If you have a particular view on what would be the most convenient option(s) I wouldn't mind contributing.

(e.g. bringing back something along the lines of `possible_values_from_str = [...]` as probably the most convenient thing, with the name being more explicit so as to be less confusing; or one step back, `PossibleValuesFromStr` as above).

---

_Comment by @epage on 2022-10-26 16:15_

Issue first as I prefer exploring on directions there. Once we have a direction, you are welcome to get a PR going.

---
