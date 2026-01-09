---
number: 4558
title: "Support non-Display types with `default_value_t`"
type: issue
state: open
author: rdelfin
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-12-16T21:03:24Z
updated_at: 2023-03-29T20:40:49Z
url: https://github.com/clap-rs/clap/issues/4558
synced_at: 2026-01-07T13:12:20-06:00
---

# Support non-Display types with `default_value_t`

---

_Issue opened by @rdelfin on 2022-12-16 21:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.29

### Describe your use case

Currently, there's no way (that I can figure out) to set a default value on a `PathBuf` using derive. You can see a minimal reproducible example [here](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=319840ec4a9e06a86ca5369193950d8d). The specific error is:
```
error[[E0277]](https://doc.rust-lang.org/stable/error-index.html#E0277): `PathBuf` doesn't implement `std::fmt::Display`
 --> src/main.rs:6:24
  |
6 |     #[arg(short, long, default_value_t = PathBuf::from("/some/default/path"))]
  |                        ^^^^^^^^^^^^^^^ `PathBuf` cannot be formatted with the default formatter; call `.display()` on it
  |
  = help: the trait `std::fmt::Display` is not implemented for `PathBuf`
  = note: call `.display()` or `.to_string_lossy()` to safely print paths, as they may contain non-Unicode data
  = note: required for `PathBuf` to implement `ToString`
```


### Describe the solution you'd like

Ideally, either logic to allow for PathBufs to display correctly with the `.display()` call, or a way of suplementing default values let you write a custom display string.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @rdelfin on 2022-12-16 21:03_

---

_Renamed from "Allow for default values with PathBufs" to "Support non-Display types with `default_value_t`" by @epage on 2022-12-16 21:28_

---

_Label `A-derive` added by @epage on 2022-12-16 21:29_

---

_Label `S-waiting-on-design` added by @epage on 2022-12-16 21:29_

---

_Comment by @epage on 2022-12-16 21:30_

If your default is literally `PathBuf::from("/some/default/path")`, then you can workaround this with `default_value = "/some/default/path"`.

---

_Comment by @rdelfin on 2022-12-16 23:09_

AH, thanks @epage! I assume `default_value` just calls `from()` on the value provided?
Either way, this resolves the issue for me, but sounds like there could be a use for the broader problem. Leave it to you all if you want to leave it open

---

_Comment by @epage on 2022-12-17 00:57_

Yeah, I've been wanting to do something about this but it appears we don't have an issue for this, so now we do!

Most likely this will require deref coercion, like `value_builder!` to know whether we can use `Display` or whether we need another way to eventually get it into a `clap::builder::OsStr`.

---

_Comment by @cdaringe on 2023-03-23 05:29_

this thread was critical for me! the clap docs & guide didn't call out `default_value = ...`, just `default_value_t = ...`

thanks!

---

_Comment by @epage on 2023-03-23 12:15_

@cdaringe the docs mention both but they might not be the most obvious.  #4090 is about trying to find ways to improve that.

---

_Comment by @nickeb96 on 2023-03-29 20:18_

Is it possible to specify a default value that doesn't get passed through a parser?  I have something like:

```rust
#[derive(Parser)]
struct Args {
    #[arg(long, value_parser = thing_parser, default_value_t = THING_CONST)]
    thing: Thing,
}

fn thing_parser(s: &str) -> Result<Thing, Error> { ... }
```

Unfortunately this takes `THING_CONST` (which is already type `Thing`) and converts it into a string, passes that string to `thing_parser` to be reconstructed as a `Thing`, and then uses that as the default value for `args.thing`.

The example is a bit over-simplified, but basically I've run into situations where I can't implement `fmt::Display` for `Thing` because it's from an external library.  Or it has a display implementation that can't reliably be parsed.

The workarounds I've used are not pretty.  I've tried making wrapper types for `Thing` that allow it to be successfully turned into a string and back again, but this is suboptimal.  I also have to unwrap the wrappers everywhere they're used.  Another workaround is to have two argument structs, `RawArgs` and `RealArgs`.  `RawArgs` has `#[derive(Parser)]` on it and only has basic `String`/`Option<String>` types.  `RealArgs` is constructed from `RawArgs` and is what gets used everywhere.  I feel like this defeats the purpose of using the derive interface.

---

_Comment by @epage on 2023-03-29 20:40_

>  I've run into situations where I can't implement fmt::Display for Thing because it's from an external library. 

Within the builder API, the primary value add of defaults is to display them in help which requires `Display`.

For derive users, the value proposition is a little different because someone just might prefer their struct to have `T` rather than `Option`.  However, the downsides to using an `Option` don't seem too high, so this is a motivation but not a strong one

With that said, one possible way of solving this is to expand on [our auto-detection of `Display` vs a custom `OsStr`-based trait](https://github.com/clap-rs/clap/issues/4558#issuecomment-1355886787), we could also detect if neither is supported and bypass the regular default mechanism.  I haven't fully thought this through on whether we can get away with only calling the user's `default_value_t` expression once or whether we would need to call it twice (builder and accessor are in separate functions)

> Or it has a display implementation that can't reliably be parsed.

This is somewhat similar to the last point, the user seeing the default should probably be able to pass it in manually.

---

_Referenced in [clap-rs/clap#1695](../../clap-rs/clap/issues/1695.md) on 2023-10-25 03:03_

---

_Referenced in [bytecodealliance/wasm-tools#2216](../../bytecodealliance/wasm-tools/pulls/2216.md) on 2025-06-03 16:09_

---
