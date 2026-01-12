```yaml
number: 4983
title: "Documentation example for `TypedValueParser` uses non-public method call"
type: issue
state: closed
author: stmcginnis
labels:
  - C-bug
assignees: []
created_at: 2023-06-22T14:20:31Z
updated_at: 2023-06-22T15:17:08Z
url: https://github.com/clap-rs/clap/issues/4983
synced_at: 2026-01-12T16:14:16Z
```

# Documentation example for `TypedValueParser` uses non-public method call

---

_@stmcginnis_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.70.0 (90c541806 2023-05-31)

### Clap Version

4.3.5

### Minimal reproducible code

See example from https://docs.rs/clap/latest/clap/builder/trait.TypedValueParser.html#example

Specifically these lines:

```rust
let inner = clap::value_parser!(u32);
let val = inner.parse_ref(cmd, arg, value)?;
```

In my particular case, trying to use this example and doing:

```rust
let inner = clap::value_parser!(String);
let val = inner.parse_ref(cmd, arg, value)?; // error: method `parse_ref` is private
```

### Steps to reproduce the bug with the above code

Attempt to implement the given example in a project using `clap` (as opposed to directly in the `clap` source).

### Actual Behaviour

The example given shows calling `parse_ref()`, which is defined as `pub(crate)`, so any attempt to use this as just a consumer of the crate will give an error.

This was [brought up in a discussion](https://github.com/clap-rs/clap/discussions/3856#discussioncomment-2986346), but I think the detail was missed.

### Expected Behaviour

Custom `TypeValueParsers` in consuming code could benefit from being able to call this `pub(crate)` call to perform basic argument parsing before applying custom logic.

I think either the function should be just made `pub`, or the example in the documentation should be updated to provide an alternative method for performing this base level of parsing before the custom parsing logic.

### Additional Context

_No response_

### Debug Output

N/A

---

_Label `C-bug` added by @stmcginnis on 2023-06-22 14:20_

---

_Comment by @epage on 2023-06-22 15:12_

`clap::value_parser!(u32);` returns a type that implements `TypedValueParser`, so `TypedValueParser::parse_ref` is being used.

With `clap::value_parser!(String)`, a `ValueParser` is being returned (which avoids allocations for such a common type) but that means it doesn't implement `TypedValueParser`.

The problem is `ValueParser::parse_ref` won't quite do what you want, giving you back a `AnyValue` rather than a `String`.

What you likely want to do is to use `clap::builder::StringValueParser` instead of `clap::value_parser!(String)` which will give you access to `TypedValueParser::parse_ref`.

---

_Comment by @stmcginnis on 2023-06-22 15:17_

Great, that appears to work as expected. Thanks for the explanation.

And thanks for maintaining `clap` - awesome project!

---

_Closed by @stmcginnis on 2023-06-22 15:17_

---
