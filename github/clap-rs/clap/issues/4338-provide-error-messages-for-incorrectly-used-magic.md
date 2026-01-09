---
number: 4338
title: Provide error messages for incorrectly used magic attributes
type: issue
state: closed
author: stevenengler
labels:
  - C-bug
  - S-triage
assignees: []
created_at: 2022-10-03T16:26:21Z
updated_at: 2022-10-31T17:41:18Z
url: https://github.com/clap-rs/clap/issues/4338
synced_at: 2026-01-07T13:12:20-06:00
---

# Provide error messages for incorrectly used magic attributes

---

_Issue opened by @stevenengler on 2022-10-03 16:26_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0

### Clap Version

4.0.8

### Minimal reproducible code

```rust
fn main() {
    let args = Args::parse();
    println!("{args:?}");
}

#[derive(clap::Parser, Debug)]
#[command(next_display_order(None))]
struct Args {
    #[arg(long)]
    c: bool,

    #[arg(long)]
    b: bool,

    #[arg(long)]
    a: bool,
}
```

### Steps to reproduce the bug with the above code

```
cargo run -- --help
```

### Actual Behaviour

```
Usage: example [OPTIONS]

Options:
      --c
      --b
      --a
  -h, --help  Print help information
```

### Expected Behaviour

```
Usage: example [OPTIONS]

Options:
      --a
      --b
      --c
  -h, --help  Print help information
```

### Additional Context

I eventually tried `command(next_display_order = None)` instead of `command(next_display_order(None))`, which then produces the expected result (alphabetically sorted args in help output). The derive documentation has a small section on "magic attributes" which says:

> - `method = arg` can only be used for methods which take only one argument.
> - `method(arg1, arg2)` can be used with any method.

So I would have expected `command(next_display_order = None)` and `command(next_display_order(None))` to behave the same. But the documentation later says:

> Magic attributes are more constrained in the syntax they support, usually just `<attr> = <value>` though some use `<attr>(<value>)` instead. See the specific magic attributes documentation for details. This allows users to access the raw behavior of an attribute via `<attr>(<value>)` syntax.
> [...]
> - next_display_order: `Command::next_display_order`

I find this confusing, and I don't know what syntax I'm supposed to use here, and what the expected behaviour is. But I think if `command(next_display_order(None))` is not supposed to be used here, then clap should output some error message.

### Debug Output

_No response_

---

_Label `C-bug` added by @stevenengler on 2022-10-03 16:26_

---

_Renamed from "Provide better error messages for incorrectly used magic attributes" to "Provide error messages for incorrectly used magic attributes" by @stevenengler on 2022-10-03 16:29_

---

_Comment by @epage on 2022-10-04 16:20_

>  But I think if command(next_display_order(None)) is not supposed to be used here, then clap should output some error message.

If you run `cargo expand`, you will see that we are doing exactly what you told us to do.  The problem is that the raw method is being applied after all of the arguments.

There isn't anything invalid for us to error on.

Now, looking to improve the raw/magic documentation and/or removing the overriding of magic to be raw are things we can look into

---

_Comment by @epage on 2022-10-04 16:22_

> The derive documentation has a small section on "magic attributes" which says:

Note that the quoted section is under "raw attributes", not under "magic attributes".

---

_Comment by @epage on 2022-10-04 16:22_

> I find this confusing, and I don't know what syntax I'm supposed to use here, and what the expected behaviour is.

Generally, I would recommend using `=` unless you can't as that is most likely to give you the behavior you expect.

---

_Label `S-triage` added by @epage on 2022-10-04 16:23_

---

_Comment by @stevenengler on 2022-10-31 17:41_

Thanks for the reply. I'll close this if it's the expected behaviour, but I do find it confusing when I need to understand the internal workings of Clap in order to understand how to use the user-facing API. Maybe some documentation examples showing how the derive macros expand would be helpful.

---

_Closed by @stevenengler on 2022-10-31 17:41_

---
