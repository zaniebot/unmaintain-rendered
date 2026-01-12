```yaml
number: 4617
title: try_update_from returns an Error MissingRequiredArgument for argument with existing values
type: issue
state: closed
author: nh13
labels:
  - C-bug
  - E-easy
  - A-derive
assignees: []
created_at: 2023-01-09T22:36:25Z
updated_at: 2023-01-10T03:47:54Z
url: https://github.com/clap-rs/clap/issues/4617
synced_at: 2026-01-12T16:14:16Z
```

# try_update_from returns an Error MissingRequiredArgument for argument with existing values

---

_@nh13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.66.0

### Clap Version

4.0.32

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug, Clone)]
#[clap(name = "tool", version = "0.0.1", about="short", long_about="long", term_width=0)]

pub struct Opts {
  #[clap(long, short='f', required=true)]
  pub foo: usize,
  #[clap(long, short='b', required=true)]
  pub bar: usize,
}

pub fn main() {
  let mut opts = Opts {
      foo: 0,
      bar: 1,
  };
  println!("initial opts: {:?}", opts);
  
  let argv = vec!["tool", "--bar", "2"];
  
  match opts.try_update_from(argv.into_iter()) {
      Ok(()) => println!("OK: {:?}", opts),
      Err(err) => println!("ERR: {:?}", err)
  }
}
```


### Steps to reproduce the bug with the above code

Please see: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=43678edf67d7e39b179c141c55d2036d

### Actual Behaviour

`try_update_from` returns an error of kind `MissingRequiredArgument`

### Expected Behaviour

`try_update_from` should succeed as `foo` already set in `opt`

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @nh13 on 2023-01-09 22:36_

---

_Comment by @epage on 2023-01-09 22:53_

In `update_from`, we skip applying our implicit `required = true` but we are not overriding the users explicit `required = true` which we should

---

_Label `E-easy` added by @epage on 2023-01-09 22:53_

---

_Label `A-derive` added by @epage on 2023-01-09 22:53_

---

_Referenced in [clap-rs/clap#4618](../../clap-rs/clap/pulls/4618.md) on 2023-01-10 03:09_

---

_Closed by @epage on 2023-01-10 03:47_

---

_Referenced in [Singular-Genomics/singular-demux#33](../../Singular-Genomics/singular-demux/pulls/33.md) on 2023-01-10 16:06_

---

_Referenced in [clap-rs/clap#4974](../../clap-rs/clap/issues/4974.md) on 2023-06-19 17:18_

---
