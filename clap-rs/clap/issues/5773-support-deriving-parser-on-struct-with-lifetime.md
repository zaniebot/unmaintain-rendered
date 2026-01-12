```yaml
number: 5773
title: "Support deriving `Parser` on struct with lifetime and automatically parse `Cow<'_, T>` into `Cow::Owned` "
type: issue
state: open
author: jeertmans
labels:
  - C-enhancement
  - E-hard
  - A-derive
assignees: []
created_at: 2024-10-10T16:00:45Z
updated_at: 2024-10-10T16:11:46Z
url: https://github.com/clap-rs/clap/issues/5773
synced_at: 2026-01-12T16:14:17Z
```

# Support deriving `Parser` on struct with lifetime and automatically parse `Cow<'_, T>` into `Cow::Owned` 

---

_@jeertmans_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

v4.5.19

### Describe your use case

Currently, as investigated and discussed in #5772, it seems impossible to derive `Parser` on a struct that has fields with `Cow<'_, T>` where the lifetime isn't `'static`.

This happens, e.g., when one wants to reuse the same code to both create CLI and also structure available in the public API of the crate. E.g., see code below (that does not compile):

```rust
use clap::Parser;
use std::borrow::Cow;

// Error type is not important
fn parse_cow_str(string: &str) -> Result<Cow<'static, str>, std::io::Error>{
    Ok(Cow::Owned(string.to_owned()))
}

#[derive(Parser)]
pub struct Cli<'source> {
    #[arg(long, value_parser = clap::builder::ValueParser::new(parse_cow_str))]
    pub text: Cow<'source, str>,
}

fn main() {
    let manual_cli = Cli {
        text: "here is some text".into()
    };

    assert!(matches!(manual_cli.text, Cow::Borrowed(_)));

    let cli = Cli::parse();

    assert!(matches!(cli.text, Cow::Owned(_)));
}
```

### Describe the solution you'd like

When parsing from args, Clap should be capable to choose the `Cow::Owned` variant, and not care about the lifetime duration of the fields.

Moreover, it would be nice that Clap provides a default parser for `Cow<'_, T>` if `<T as ToOwned>::Owned` is a type that could be parsed automatically be Clap.

### Alternatives, if applicable

The only alternative I think of is currently to create a new structure, e.g., `CliOwned`, derive `Parser` on it, and implement `From<CliOwned> for Cli`, but this possibly means a lot of duplicate code, especially documentation strings that I prefer to reuse to avoid errors when copy/pasting.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @jeertmans on 2024-10-10 16:00_

---

_Label `E-hard` added by @epage on 2024-10-10 16:11_

---

_Label `A-derive` added by @epage on 2024-10-10 16:11_

---

_Referenced in [jeertmans/languagetool-rust#124](../../jeertmans/languagetool-rust/pulls/124.md) on 2024-10-11 07:30_

---
