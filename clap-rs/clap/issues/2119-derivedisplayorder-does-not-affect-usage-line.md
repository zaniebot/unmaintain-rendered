---
number: 2119
title: DeriveDisplayOrder does not affect usage line
type: issue
state: closed
author: AndreKR
labels:
  - C-bug
assignees: []
created_at: 2020-08-29T20:25:03Z
updated_at: 2020-08-30T07:06:18Z
url: https://github.com/clap-rs/clap/issues/2119
synced_at: 2026-01-10T01:27:13Z
---

# DeriveDisplayOrder does not affect usage line

---

_Issue opened by @AndreKR on 2020-08-29 20:25_

### Code

```rust
use clap::{App, AppSettings, Arg};

fn main(){
    let _matches = App::new("Example")
        .arg(Arg::with_name("charlie")
            .long("charlie")
            .value_name("foo")
            .required(true))
        .arg(Arg::with_name("alfa")
            .long("alfa")
            .value_name("foo")
            .required(true))
        .arg(Arg::with_name("bravo")
            .long("bravo")
            .value_name("foo")
            .required(true))
        .setting(AppSettings::ArgRequiredElseHelp)
        .setting(AppSettings::DeriveDisplayOrder)
        .get_matches();
}
```

### Steps to reproduce the issue

Run the program above.

[Playground link](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=e9042ba2f5c3054bcab5d7390d4a5a24)

### Version

* **Rust**: 1.46.0
* **Clap**: (I don't know how to find out on the playground.)

### Actual Behavior Summary

```text
Example 

USAGE:
    playground --alfa <foo> --bravo <foo> --charlie <foo>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
        --charlie <foo>    
        --alfa <foo>       
        --bravo <foo>   
```

### Expected Behavior Summary

I would expect the option to be in declaration order in the `USAGE` line as well, i.e.:

```text
USAGE:
    playground --charlie <foo> --alfa <foo> --bravo <foo>
```

This might be intentional but I can't see the reason why, so I think it's just an oversight. It's easy to overlook because arguments are only printed in the usage line when they are `required`.


---

_Label `T: bug` added by @AndreKR on 2020-08-29 20:25_

---

_Comment by @pksunkara on 2020-08-30 07:06_

This is fixed on master recently.

---

_Closed by @pksunkara on 2020-08-30 07:06_

---
