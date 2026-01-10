---
number: 3287
title: Using an enum for the default_value or the possible_values
type: issue
state: closed
author: Vagelis-Prokopiou
labels:
  - C-bug
  - A-derive
  - S-duplicate
assignees: []
created_at: 2022-01-12T11:04:51Z
updated_at: 2022-01-12T17:13:21Z
url: https://github.com/clap-rs/clap/issues/3287
synced_at: 2026-01-10T01:27:38Z
---

# Using an enum for the default_value or the possible_values

---

_Issue opened by @Vagelis-Prokopiou on 2022-01-12 11:04_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.0.6

### Minimal reproducible code

```rust
use std::str::FromStr;
use clap::{Parser};


#[derive(Debug)]
pub enum TranslationProvider { DeepL, Amazon, Google }

impl std::fmt::Display for TranslationProvider {
    fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
        match self {
            TranslationProvider::DeepL => { write!(f, "deepl") }
            TranslationProvider::Amazon => { write!(f, "amazon") }
            TranslationProvider::Google => { write!(f, "google") }
        }
    }
}

impl FromStr for TranslationProvider {
    type Err = String;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        if s == "deepl" { return Ok(Self::DeepL); }
        if s == "amazon" { return Ok(Self::Amazon); }
        if s == "google" { return Ok(Self::Google); }
        return Err("This translation provider is not valid".to_owned());
    }
}

/// File translator using DeepL
#[derive(Parser, Debug)]
#[clap(about, version, author)]
struct Args {
    /// The translation provider to use
    #[clap(
    long,
    default_value(&TranslationProvider::DeepL.to_string().as_str()),
    possible_values([
    clap::PossibleValue::new(TranslationProvider::DeepL.to_string().as_str()),
    ]))]
    translation_provider: TranslationProvider,
}

fn main() {
    let args = Args::parse();
}
```


### Steps to reproduce the bug with the above code

cargo run -- --help

### Actual Behaviour

I get a build error:

```
error[E0716]: temporary value dropped while borrowed
  --> src/main.rs:38:20
   |
32 | #[derive(Parser, Debug)]
   |          ------
   |          |
   |          lifetime `'b` defined here
   |          argument requires that borrow lasts for `'b`
...
35 |     /// The translation provider to use
   |                                       - temporary value is freed at the end of this statement
...
38 |     default_value(&TranslationProvider::DeepL.to_string().as_str()),
   |                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ creates a temporary which is freed while still in use

error[E0716]: temporary value dropped while borrowed
  --> src/main.rs:40:30
   |
32 | #[derive(Parser, Debug)]
   |          ------
   |          |
   |          lifetime `'b` defined here
   |          argument requires that borrow lasts for `'b`
...
35 |     /// The translation provider to use
   |                                       - temporary value is freed at the end of this statement
...
40 |     clap::PossibleValue::new(TranslationProvider::DeepL.to_string().as_str()),
   |                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ creates a temporary which is freed while still in use
```

### Expected Behaviour

I think that we should be able to use the string representation of enums some way. Either as `String` or as `String.as_str()` or as `&String`.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @Vagelis-Prokopiou on 2022-01-12 11:04_

---

_Comment by @epage on 2022-01-12 15:30_

In #1041, we plan to evaluate removing the lifetime parameter, most likely with a type like `Cow<'static, str>` so we can have `&'static str` and `String` together.  This would help in your case.

We already provide [`default_value_t`](https://github.com/clap-rs/clap/tree/master/examples/derive_ref#arg-attributes) (see also the [tutorial](https://github.com/clap-rs/clap/tree/master/examples/tutorial_derive#defaults)) which resolves the first problem you had.

Personally, when an enum maps to string values, I do something like
```rust
impl TranslationProvider {
    fn as_str(self) -> &'static str {
        match self {
            ...
        }
    }
}


impl std::fmt::Display for TranslationProvider {
    fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
        self.as_str().fmt(f)
    }
}
```
This is an alternative that resolves your second lifetime issue because you can just call `as_str()` rather than `to_string().as_str()`.


Alternatively, you could `derive` or `impl` the [`ArgEnum` trait](https://github.com/clap-rs/clap/tree/master/examples/derive_ref#arg-enum-attributes) (see also the [tutorial](https://github.com/clap-rs/clap/tree/master/examples/tutorial_derive#enumerated-values)).

---

_Label `A-derive` added by @epage on 2022-01-12 15:30_

---

_Label `S-waiting-on-author` added by @epage on 2022-01-12 15:30_

---

_Comment by @Vagelis-Prokopiou on 2022-01-12 17:04_

Thanks for the feedback @epage.
Removing the lifetime parameter is the best solution to my mind too.
For now, I will try your suggestions out, or will use any other workaround.

If you need anything else from me, let me know.

This issue can be linked to the issue you mentioned, and can be closed, I suppose.

---

_Label `S-waiting-on-author` removed by @epage on 2022-01-12 17:13_

---

_Label `S-duplicate` added by @epage on 2022-01-12 17:13_

---

_Closed by @epage on 2022-01-12 17:13_

---
