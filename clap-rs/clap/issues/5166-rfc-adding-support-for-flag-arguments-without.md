```yaml
number: 5166
title: "RFC: Adding Support for Flag Arguments Without Values"
type: issue
state: closed
author: wiseaidev
labels:
  - C-enhancement
assignees: []
created_at: 2023-10-09T17:14:04Z
updated_at: 2023-10-09T17:55:04Z
url: https://github.com/clap-rs/clap/issues/5166
synced_at: 2026-01-12T16:14:16Z
```

# RFC: Adding Support for Flag Arguments Without Values

---

_@wiseaidev_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.6

### Describe your use case

## Summary

This RFC proposes adding support for flag arguments without values in this library. Flag arguments without values would allow users to specify boolean flags without explicitly providing a boolean value (e.g., `true` or `false`), making the command-line interface more user-friendly.

## Motivation

Currently, in this library, boolean flag arguments require users to specify a boolean value (`true` or `false`). This can be seen as redundant and not user-friendly, especially for flags that are meant to act as simple switches without any associated value. This RFC aims to enhance the user experience by allowing flag arguments to be set to `true` when they are present and `false` when they are not explicitly provided by the user.

### Describe the solution you'd like

## Detailed Design

To implement this feature, we propose introducing a new attribute for flag arguments called `without_value`. When this attribute is applied to a flag argument, this library will automatically set the argument's value to `true` if it is present in the command line arguments and `false` if it is not present.

### Syntax

The `without_value` attribute can be applied to a flag argument as follows:

```rust
#[arg(short = 'c', long = "ignore-case", without_value)]
ignore_case: bool,
```

Here, `without_value` is a new attribute that informs Clap to treat the `ignore-case` flag as a flag argument without requiring a value.

### Example

Consider the following code snippet using the proposed feature:

```rust
use clap::Parser;

#[derive(Parser, Debug)]
#[command(
    author = "Mahmoud Harmouch",
    version = "1.0",
    about = "A command-line find and replace utility",
    name = "Find and Replace"
)]
struct Args {
    /// Sets the input file to process
    #[arg(short = 'i', long = "input")]
    input: String,
    /// Sets the pattern to find
    #[arg(short = 'f', long = "find")]
    find: String,
    /// Sets the replacement text
    #[arg(short = 'r', long = "replace")]
    replace: String,
    /// Perform a case-insensitive search and replace
    #[arg(short = 'c', long = "ignore-case", without_value)]
    ignore_case: bool,
}

fn main() {
    // Access and use the `input`, `find`, `replace`, and `ignore-case` arguments here
    let args = Args::parse();
}
```

With this feature, the following command-line invocation would be valid:

```shell
cargo run -- -i file_name -f hello -r hi -c
```

Without this RFC, the user would need to specify `true` explicitly:

```shell
cargo run -- -i file_name -f hello -r hi -c true
```

## Drawbacks

Introducing a new attribute, `without_value`, may add some complexity to this library codebase. However, the benefit of improving the user experience by allowing more natural flag argument handling outweighs this drawback.

### Alternatives, if applicable

## Alternatives

An alternative approach to achieve similar functionality is to rely on convention rather than introducing a new attribute. For instance, this library could automatically consider a flag argument to be `true` if it is present in the command line arguments and `false` if it is absent. However, using an explicit attribute provides better clarity and flexibility.

### Additional Context

## Implementation Plan

1. Introduce the `without_value` attribute in this library's codebase.
2. Update the parser to recognize this attribute and handle flag arguments accordingly.
3. Ensure that existing functionality remains unchanged for flag arguments with values.
4. Add appropriate tests to validate the behavior of flag arguments with and without values.
5. Update the documentation to include the new attribute and its usage.

## Future Considerations

This feature can significantly improve the user experience of command-line interfaces built with Clap, especially when dealing with boolean flags. In the future, it may lead to more user-friendly and concise command-line applications. Additionally, this feature aligns with the principle of least astonishment, making the library more intuitive for users.

## Conclusion

The addition of support for flag arguments without values in this library is a valuable enhancement that simplifies the command-line interface for users. It reduces redundancy and improves the user experience when working with boolean flags. This RFC proposes the introduction of the `without_value` attribute to achieve this functionality, ensuring that existing functionality remains intact while providing more natural flag argument handling.

---

_Label `C-enhancement` added by @wiseaidev on 2023-10-09 17:14_

---

_Comment by @epage on 2023-10-09 17:21_

I feel I'm missing something as what you are proposing sounds like functionality we already have today, see https://docs.rs/clap/latest/clap/_derive/_tutorial/chapter_2/index.html#flags

---

_Comment by @wiseaidev on 2023-10-09 17:32_

Thanks for the response, @epage! This works, but when making the argument optional:

```rust
use clap::Parser;

#[derive(Parser, Debug)]
#[command(
    author = "Mahmoud Harmouch",
    version = "1.0",
    about = "A command-line find and replace utility",
    name = "Find and Replace"
)]
struct Args {
    /// Sets the input file to process
    #[arg(short = 'i', long = "input")]
    input: String,
    /// Sets the pattern to find
    #[arg(short = 'f', long = "find")]
    find: String,
    /// Sets the replacement text
    #[arg(short = 'r', long = "replace")]
    replace: String,
    /// Perform a case-insensitive search and replace
    #[arg(short = 'c', long = "ignore-case")]
    ignore_case: Option<bool>,
}

fn main() {
    // Access and use the `input`, `find`, `replace`, and `ignore-case` arguments here
    let args = Args::parse();
}
```

 It returns:

```sh
$ cargo run -- -i file_name -f hello -r hi -c

error: a value is required for '--ignore-case <IGNORE_CASE>' but none was supplied
  [possible values: true, false]
```

Is that expected?

---

_Comment by @epage on 2023-10-09 17:41_

What is your intention with adding `Option`?

`Option` hasn't generally made sense for flags, so we've left it completely for options that take values.  The "magic" type behavior is described at https://docs.rs/clap/latest/clap/_derive/index.html#arg-types.  #4626 is for discussing improving the system.

---

_Comment by @wiseaidev on 2023-10-09 17:55_

Thanks for your reply; I understand your perspective, and I am going to close this issue in favor of the mentioned discussion.

---

_Closed by @wiseaidev on 2023-10-09 17:55_

---
