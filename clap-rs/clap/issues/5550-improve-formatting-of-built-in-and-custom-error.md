---
number: 5550
title: Improve formatting of built-in and custom error messages being appended to each other
type: issue
state: open
author: mahor1221
labels:
  - C-enhancement
  - M-breaking-change
  - A-help
assignees: []
created_at: 2024-06-25T13:34:22Z
updated_at: 2025-09-04T06:53:53Z
url: https://github.com/clap-rs/clap/issues/5550
synced_at: 2026-01-10T01:28:13Z
---

# Improve formatting of built-in and custom error messages being appended to each other

---

_Issue opened by @mahor1221 on 2024-06-25 13:34_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.7

### Describe your use case

I return multi-line errors in some parsers, which causes minor formatting issues when using `RichFormatter`. For example:

```rust
#[derive(Parser)]
pub struct Cli {
    ...
    #[arg(long, short, value_parser = TransitionScheme::from_str)]
    #[arg(value_name = "TIME-TIME | DEGREE:DEGREE")]
    pub scheme: Option<TransitionScheme>,
    ...
}
```
Current format:
```yaml
$: cargo run -- --scheme asdf:ghjk
error: invalid value 'asdf:ghjk' for '--scheme <TIME-TIME | DEGREE:DEGREE>': as time ranges:
- invalid format
as elevation range:
- invalid float literal (asdf)
- invalid float literal (ghjk)

For more information, try '--help'.
```

Desired format:

```yaml
$: cargo run -- --scheme asdf:ghjk
error: invalid value 'asdf:asdf' for '--scheme <TIME-TIME | DEGREE:DEGREE>': # a new line is added here
as time ranges:
- invalid format
as elevation range:
- invalid float literal (asdf)
- invalid float literal (ghjk)

For more information, try '--help'.
```



### Describe the solution you'd like

Please note that I'm not very familiar with the clap's code base. Should this feature even be added or not? 

A possible solution (but most likely a breaking change) would be to check if the formatted `source` error is multi-line, and if so, add a newline character to ensure proper formatting.

Another possible solution would be to make `RichFormatter ` configurable. 

### Alternatives, if applicable

I've created a patch for my own use case here.
https://github.com/clap-rs/clap/compare/v4.5.7...mahor1221:clap:patch?expand=1 

### Additional Context

Here is the location of the issue:
https://github.com/clap-rs/clap/blob/6c6839a454c2cbfc3007e6a2b2a55dd64685771e/clap_builder/src/error/format.rs#L370

---

_Label `C-enhancement` added by @mahor1221 on 2024-06-25 13:34_

---

_Label `M-breaking-change` added by @epage on 2024-06-26 15:29_

---

_Label `A-help` added by @epage on 2024-06-26 15:29_

---

_Comment by @epage on 2024-06-26 15:30_

This is a tough situation because it happens to be a problem in your case due to the lack of alignment but in other cases it can be neutral or better the existing way.

---

_Renamed from "Improve RichFormatter's multi-line error formatting " to "Improve formatting of built-in and custom error messages being appended to each other" by @epage on 2025-08-07 20:12_

---
