---
number: 5130
title: "contains_id doesn't panic"
type: issue
state: closed
author: Avgor46
labels:
  - C-bug
assignees: []
created_at: 2023-09-19T16:50:42Z
updated_at: 2023-09-19T17:59:18Z
url: https://github.com/clap-rs/clap/issues/5130
synced_at: 2026-01-07T13:12:20-06:00
---

# contains_id doesn't panic

---

_Issue opened by @Avgor46 on 2023-09-19 16:50_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.0 (5680fa18f 2023-08-23)

### Clap Version

4.4.4

### Minimal reproducible code

```rust
fn main() {
    let matches = clap::Command::new("myapp")
        .get_matches_from(["myapp"]);

    match matches.try_contains_id("arg") {
        Ok(fl) => println!("Ok: {fl}"),
        Err(e) => println!("Error: {e}"),
    }

    println!("It must panic: {}", matches.contains_id("arg"));
}
```


### Steps to reproduce the bug with the above code

First, run with `cargo run`, then with `cargo run --release`

### Actual Behaviour

With `cargo run`:

        Error: Unknown argument or group id.  Make sure you are using the argument id and not the short or long flags

        thread 'main' panicked at 'Mismatch between definition and access of `arg`. Unknown argument or group id. 
        Make sure you are using the argument id and not the short or long flags

With `cargo run --release`:

        Ok: false
        It must panic: false


### Expected Behaviour

Imho program above should print Error: {error message} and then panic in all cases.

### Additional Context

Description says that `contains_id` panics `If id is not a valid argument or group name.` But in release mod it isn't true. Perhaps [line](https://github.com/clap-rs/clap/blob/e6e539660f36487e3521ab6835ef1381a21785a4/clap_builder/src/parser/matches/arg_matches.rs#L1267) is the reason for this behavior. If this behavior was intended, the description of the function may be misleading to users. What do you think?

### Debug Output

_No response_

---

_Label `C-bug` added by @Avgor46 on 2023-09-19 16:50_

---

_Comment by @epage on 2023-09-19 16:57_

It is intentional that our asserts are debug only as we don't want the extra bookkeeping to slow down applications.

Without any extra motivation given, I'm inclined to close this.  If there is a reason we should reconsider this, let us know!

---

_Closed by @epage on 2023-09-19 16:57_

---

_Comment by @Avgor46 on 2023-09-19 17:17_

According to the documentation, `contains_id` should panic if an invalid argument is given, but in release mode it does not panic. Am I missing something here? @epage

---

_Comment by @epage on 2023-09-19 17:37_

The distinction is trivial since your code should work on both but I'm updating them to do clarify it.

---

_Comment by @Avgor46 on 2023-09-19 17:59_

Ok, thank you

---
