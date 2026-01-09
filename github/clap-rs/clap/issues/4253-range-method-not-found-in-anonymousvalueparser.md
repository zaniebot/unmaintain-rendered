---
number: 4253
title: "range method not found in `_AnonymousValueParser(usize)`"
type: issue
state: open
author: pkolaczk
labels:
  - C-bug
  - A-builder
  - S-waiting-on-design
assignees: []
created_at: 2022-09-24T08:40:28Z
updated_at: 2022-09-28T16:22:33Z
url: https://github.com/clap-rs/clap/issues/4253
synced_at: 2026-01-07T13:12:20-06:00
---

# range method not found in `_AnonymousValueParser(usize)`

---

_Issue opened by @pkolaczk on 2022-09-24 08:40_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

4.0.0-rc2

### Minimal reproducible code

```rust
#[derive(clap::Parser, Debug, Default)]
#[command(disable_version_flag = true)]
struct MyApp {
    #[arg(value_parser = clap::value_parser!(usize).range(1..))]
    pub foo: Option<usize>
}

fn main() {
}
```


### Steps to reproduce the bug with the above code

cargo check

### Actual Behaviour

```
error[E0599]: no method named `range` found for struct `_AnonymousValueParser` in the current scope
 --> src/test.rs:4:53
  |
4 |     #[arg(value_parser = clap::value_parser!(usize).range(1..))]
  |                                                     ^^^^^ method not found in `_AnonymousValueParser`

```

### Expected Behaviour

Should compile fine

### Additional Context

If I replace the `usize` type in value_parser! with `u64`, then it compiles fine, however I'm not sure if this is correct, because that's a different type than the type used in the field.

### Debug Output

_No response_

---

_Label `C-bug` added by @pkolaczk on 2022-09-24 08:40_

---

_Comment by @anshulrgoyal on 2022-09-24 21:37_

@pkolaczk `clap::value_parser!` returns `RangedU64ValueParser` for `u64` therefore range function is available, but usize returns `_AnonymousValueParser` hence the compilation error.

If there is an implementation for `usize` then it will work.


```rust
impl ValueParserFactory for usize {
    type Parser = RangedU64ValueParser<usize>;
    fn value_parser() -> Self::Parser {
        RangedU64ValueParser::<usize>::new()
    }
}


```


---

_Comment by @pkolaczk on 2022-09-25 10:35_

Such implementation can be added only by `clap`. I cannot add it by myself, because of foreign-crate restriction.

---

_Comment by @anshulrgoyal on 2022-09-25 13:49_

@pkolaczk I was suggesting adding the implementation in clap only. I don't know the reason it is not already present.

---

_Comment by @epage on 2022-09-26 15:49_

I wish I could more generically implement this...

clap does use `try_into` to go from `u64` to `T` (`usize` in this case).  My main question is if converting from u64 to usize is "good enough" or if we should have usize-native value parser.

---

_Label `S-waiting-on-design` added by @epage on 2022-09-26 15:49_

---

_Label `A-builder` added by @epage on 2022-09-26 15:49_

---

_Comment by @epage on 2022-09-28 16:22_

btw #3895 and #4034 are related PRs

---

_Renamed from "range method not found in _AnonymousValueParser" to "range method not found in `_AnonymousValueParser(usize)`" by @epage on 2022-09-28 16:22_

---

_Referenced in [paritytech/polkadot#6100](../../paritytech/polkadot/pulls/6100.md) on 2022-10-06 15:29_

---
