---
number: 6147
title: Text with ANSI escape sequences is incorrectly wrapped
type: issue
state: closed
author: ongardie
labels:
  - C-bug
  - S-triage
assignees: []
created_at: 2025-10-10T23:29:19Z
updated_at: 2025-10-13T16:10:54Z
url: https://github.com/clap-rs/clap/issues/6147
synced_at: 2026-01-07T13:12:20-06:00
---

# Text with ANSI escape sequences is incorrectly wrapped

---

_Issue opened by @ongardie on 2025-10-10 23:29_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.90.0

### Clap Version

master

### Minimal reproducible code

Here are some basic unit tests for `StyledStr::wrap()`. The `wrap_styled` test fails both assertions:

```rust
#[cfg(test)]
 mod tests {
     use super::*;
 
     #[test]
     #[cfg(feature = "wrap_help")]
     fn wrap_unstyled() {
         let mut unstyled = StyledStr::from(color_print::cstr!("12345 12345 12345 12345"));
         unstyled.wrap(20);
         assert_eq!("12345 12345 12345\n12345", unstyled.to_string());
     }
 
     #[test]
     #[cfg(feature = "wrap_help")]
     fn wrap_styled() {
         let mut styled = StyledStr::from(color_print::cstr!(
             "<bold>12345</bold> <bold>12345</bold> <bold>12345</bold> <bold>12345</bold>"
         ));
         styled.wrap(20);
         assert_eq!(
             color_print::cstr!(
                 "<bold>12345</bold> <bold>12345</bold> <bold>12345</bold>\n<bold>12345</bold>"
             ),
             styled.as_styled_str()
         );
         assert_eq!("12345 12345 12345\n12345", styled.to_string());
     }
 }
```


### Steps to reproduce the bug with the above code

- Copy these tests into `clap_builder/src/builder/styled_str.rs`.
- Enable the `wrap_help` feature.
- Run the tests.

### Actual Behaviour

When I provide `after_long_help` containing ANSI escape sequences with `wrap_text` enabled, the wrapping is jagged.

### Expected Behaviour

It should wrap the same as if the ANSI escape sequences weren't there (except the sequences should be preserved).

### Additional Context

It looks like the code that's there intended to handle this, but something is off.

This could probably use additional unit tests, too, including some that have line breaks in the input.

### Debug Output

_No response_

---

_Label `C-bug` added by @ongardie on 2025-10-10 23:29_

---

_Label `S-triage` added by @ongardie on 2025-10-10 23:29_

---

_Referenced in [clap-rs/clap#6149](../../clap-rs/clap/pulls/6149.md) on 2025-10-13 15:51_

---

_Closed by @epage on 2025-10-13 16:10_

---
