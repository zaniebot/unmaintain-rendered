---
number: 5885
title: Fallback for ValueEnum
type: issue
state: open
author: ModProg
labels:
  - C-enhancement
assignees: []
created_at: 2025-01-19T23:18:16Z
updated_at: 2025-01-20T18:27:35Z
url: https://github.com/clap-rs/clap/issues/5885
synced_at: 2026-01-10T01:28:18Z
---

# Fallback for ValueEnum

---

_Issue opened by @ModProg on 2025-01-19 23:18_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.26

### Describe your use case

I'd like to have support for a fallback option when using `ValueEnum`.

### Describe the solution you'd like

 When `derive(ValueEnum)` is used I'd like to have a variant e.g., `Other(String)`, annotated with something like `#[clap(other|fallback)]` to contain the literal value if none of the enum variants matches.

### Alternatives, if applicable

Using `value_parser` to and `skip` like so:
```rs
#[derive(Debug, Parser)]
struct Args {
    #[clap(value_parser = enum_parser)]
    arg: Enum,
}

#[derive(ValueEnum, Clone, Debug)]
enum Enum {
    A,
    B,
    #[clap(skip)]
    Other(String),
}

fn enum_parser(value: &str) -> Result<Enum, Infallible> {
    Ok(Enum::from_str(value, true).unwrap_or_else(|_| Enum::Other(value.to_owned())))
}
```

### Additional Context

_No response_

---

_Label `C-enhancement` added by @ModProg on 2025-01-19 23:18_

---

_Referenced in [clap-rs/clap#5886](../../clap-rs/clap/pulls/5886.md) on 2025-01-20 01:24_

---

_Comment by @ModProg on 2025-01-20 01:25_

I tried to implement a solution, but it turns out this is not a purely proc-macro change.

This would require to either add fallback to the `ValueEnum` trait or make `EnumValueParser` respect `ValueEnum`'s `from_str` implementation.

---

_Comment by @ModProg on 2025-01-20 01:26_

Not sure what your preferred implementation would be here, should you want to implement this.

---

_Comment by @epage on 2025-01-20 15:52_

Serde calls such an attribute [`other`].

Adding to `ValueEnum` would be a breaking change.  Any thoughts on what that would look like?  Changing `EnumValueParser` might be able to be considered a breaking change.

An important question for this is also how to handle different types.  One option is to only accept `String` and related types (`OsString`, `PathBuf`).  Another is to use `value_parser!` on the variant's type and allow `other = value_parser`.

We'd also need to look at the impact on dynamic completions in case it affects the design at all.

---

_Comment by @ModProg on 2025-01-20 17:10_

`other` might be the better name then.

I'd have gone for only String based types for now, though I could see someone wanting to use, e.g., a number. Though then you might want to have multiple `fallback` or `others`, this could be expanded on in the future, if we design the change to `EnumValueParser` or `ValueEnum` correctly.

Currently, `EnumValueParser` is just ignoring the implementation of `ValueEnum::from_str` I don't know if this is intentional. Changing that is a breaking change I guess, though it is one that is quite unlikely to affect anyone, as `ValueEnum::from_str` will be the default implementation for most, which matches the implementation in `EnumValueParser` AFAICT.

Adding a new function with a default implementation to `ValueEnum` would also only be a breaking change for those that manually implemented parsing for generic `ValueEnum`s which is probably also not very common.

For dynamic completions I'd have said it should be considered `skipped` as it is not a variant that could be completed (at least for the simple String case, if we supported `value_parser` then there could be completions).

To support completions for the `other` variant, I think we'd definitely need to change the API of `ValueEnum`.

---

_Comment by @epage on 2025-01-20 18:07_

> Currently, EnumValueParser is just ignoring the implementation of ValueEnum::from_str I don't know if this is intentional. Changing that is a breaking change I guess, though it is one that is quite unlikely to affect anyone, as ValueEnum::from_str will be the default implementation for most, which matches the implementation in EnumValueParser AFAICT.

I don't remember the details but it looks like `EnumValueParser` offers better error messages.

---

_Comment by @ModProg on 2025-01-20 18:27_

> I don't remember the details but it looks like EnumValueParser offers better error messages.

Yeah, `ValueEnum` returns a `String` error. To keep the better error messages, one approach could be to just call `ValueEnum::from_str` and ignore the error message:
https://github.com/clap-rs/clap/blob/f89134d5e11cbd779a48bff6efec897e9cb55ecc/clap_builder/src/builder/value_parser.rs#L1119-L1126

```rs
let value = E::from_str(value, ignore_case).map_err(|| {
```

The other would be to move the generating of a proper error inside `ValueEnum` but that would require either adding a new function or changing `from_str`.

---
