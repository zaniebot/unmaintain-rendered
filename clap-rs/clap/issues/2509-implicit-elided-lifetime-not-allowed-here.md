---
number: 2509
title: implicit elided lifetime not allowed here
type: issue
state: closed
author: vbmade2000
labels:
  - C-bug
assignees: []
created_at: 2021-05-29T12:11:47Z
updated_at: 2021-05-29T20:28:15Z
url: https://github.com/clap-rs/clap/issues/2509
synced_at: 2026-01-10T01:27:20Z
---

# implicit elided lifetime not allowed here

---

_Issue opened by @vbmade2000 on 2021-05-29 12:11_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.54.0-nightly

### Clap Version

2.33.3

### Minimal reproducible code

```rust
use clap::{App, Arg, ArgMatches};


async fn print_values_from_args(args: ArgMatches) {
    // Print all args here
}

#[tokio::main]
async fn main() {
        let matches = App::new("test cli")
        .arg(
            Arg::with_name("config")
                .short("c")
                .long("config")
                .value_name("config")
                .help("Configuration file containing data to send")
                .takes_value(true)
                .required(true),
        )
        .get_matches();

        print_values_from_args(matches);
}
```


### Steps to reproduce the bug with the above code

cargo build

### Actual Behaviour
```

error[E0726]: implicit elided lifetime not allowed here
 --> src/main.rs:4:39
  |
4 | async fn print_values_from_args(args: ArgMatches) {
  |                                       ^^^^^^^^^^- help: indicate the anonymous lifetime: `<'_>`

error: aborting due to previous error

error: could not compile `clap_test`

To learn more, run the command again with --verbose.
```


### Expected Behaviour

It should compile without any issue.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @vbmade2000 on 2021-05-29 12:11_

---

_Comment by @vbmade2000 on 2021-05-29 12:42_

This may not be actually a bug but I didn't see it fit in other categories. This is indeed an issue for me as a Rust newbie. 

---

_Closed by @pksunkara on 2021-05-29 20:28_

---

_Locked by @clap-rs on 2021-05-29 20:28_

---
