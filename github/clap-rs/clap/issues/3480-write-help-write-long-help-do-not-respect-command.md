---
number: 3480
title: write_help / write_long_help do not respect Command color settings
type: issue
state: closed
author: nicklan
labels:
  - C-bug
  - A-help
  - S-wont-fix
assignees: []
created_at: 2022-02-16T23:04:33Z
updated_at: 2022-02-28T17:48:15Z
url: https://github.com/clap-rs/clap/issues/3480
synced_at: 2026-01-07T13:12:19-06:00
---

# write_help / write_long_help do not respect Command color settings

---

_Issue opened by @nicklan on 2022-02-16 23:04_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.1.0

### Minimal reproducible code

```rust
use std::io;
use clap::{Command, ColorChoice};

fn main() {
    let mut cmd = Command::new("myprog").color(ColorChoice::Always);

    println!("This prints color\n");
    cmd.print_help().unwrap();


    println!("\n\nThis doesn't\n");
    let mut out = io::stdout();
    cmd.write_help(&mut out).expect("failed to write to stdout");
}
```


### Steps to reproduce the bug with the above code

Just run it and see that the first prints color, but the second does not

### Actual Behaviour

No color, even though Color.Always is set

### Expected Behaviour

Should print color

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @nicklan on 2022-02-16 23:04_

---

_Renamed from "write_help / write_long_help do not respect Command color settings" to "`write_help` / `write_long_help` do not respect Command color settings" by @nicklan on 2022-02-16 23:04_

---

_Renamed from "`write_help` / `write_long_help` do not respect Command color settings" to "write_help / write_long_help do not respect Command color settings" by @nicklan on 2022-02-16 23:05_

---

_Label `A-help` added by @epage on 2022-02-16 23:43_

---

_Label `S-wont-fix` added by @epage on 2022-02-16 23:43_

---

_Comment by @epage on 2022-02-16 23:43_

This is expected because we `Auto` won't be able to detect whether we should print colors, so we print everything unstyled.

---

_Comment by @nicklan on 2022-02-17 19:58_

Okay, but I set `ColorChoice::Always`.

In general it would be nice for an application to be able to render colored help to something other than `stdout`, which isn't possible today.

Some ideas:
1. Expose the `Colorizer` (https://github.com/clap-rs/clap/blob/master/src/output/fmt.rs#L9) so users can mimic what `print_help` does.
2. Provide a `write_help_color`
3. Add a new argument to `write_help` to enable/disable color
4. Have `write_help` at least respect `ColorChoice::Always` and print color in that case.

---

_Comment by @epage on 2022-02-17 20:08_

> Have write_help at least respect ColorChoice::Always and print color in that case.

I had been thinking about this and this is one that I don't think we should do.  For example, in my applications, I do not rely on clap auto-determining whether color should be shown but instead rely on the `concolor_clap` crate to decide.  So for mine, its always set to `Always` or `Never`.  The function call needs to be independent of that.

> Expose the Colorizer (https://github.com/clap-rs/clap/blob/master/src/output/fmt.rs#L9) so users can mimic what print_help does.

Over time, we do plan to expose `Colorizer` (its not been polished enough to do so now) and give people more direct control over colors.  https://github.com/clap-rs/clap/discussions/3476#discussioncomment-2189821 is an example of these plans and relevant issues are
- https://github.com/clap-rs/clap/issues/2914
- https://github.com/clap-rs/clap/issues/2913

---

_Comment by @nicklan on 2022-02-17 20:40_

Cool, thanks. Having the direct control would solve this issue. 

Not quite clear on why you think having `write_help` respect `ColorChoice::Always` is a bad idea? Saying that other systems might try and determine it doesn't make sense. If I say "always do color" then shouldn't clap always do color?

---

_Comment by @epage on 2022-02-17 20:54_

When I specify the `ColorChoice` on the `Command`, its controlling the output of my errors, help, etc and its being set according to what `concolor` thinks my terminal is capable of.

If I were to call `write_help`, it can be for a different terminal and purpose and so setting it for one isn't necessarily relevant for the other.

---

_Comment by @nicklan on 2022-02-17 20:58_

I get that for your use case respecting Always would break things, but it seems like you're relying on inconsistent behavior. Sure, it could be that `write_help` is going somewhere else that doesn't want color, but then you shouldn't ask to always have color :). 

But I also know breaking existing things is not always an option, which was why I suggested `write_help_color` as a way to not break any existing code, but still allow writing colored help somewhere other than stdout.

---

_Closed by @epage on 2022-02-28 17:48_

---

_Referenced in [alexwlchan/dominant_colours#20](../../alexwlchan/dominant_colours/issues/20.md) on 2022-10-24 20:09_

---
