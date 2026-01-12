```yaml
number: 3838
title: "Using `default_value_ifs` with `required` returns error about argument not being provided when default is matched sometime after 3.1.12"
type: issue
state: closed
author: dsherret
labels:
  - C-bug
  - E-hard
  - A-validators
assignees: []
created_at: 2022-06-15T17:13:10Z
updated_at: 2023-08-21T18:23:19Z
url: https://github.com/clap-rs/clap/issues/3838
synced_at: 2026-01-12T16:14:15Z
```

# Using `default_value_ifs` with `required` returns error about argument not being provided when default is matched sometime after 3.1.12

---

_@dsherret_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.61

### Clap Version

3.2.5

### Minimal reproducible code

```rust
#[test]
fn default_ifs_arg_required_not_present() {
    let r = Command::new("df")
        .arg(arg!(--opt <FILE> "some arg").required(false))
        .arg(
            arg!([arg] "some arg")
                .default_value_ifs(&[
                    ("opt", Some("some"), Some("default")),
                ])
                .required(true),
        )
        .try_get_matches_from(vec!["", "--opt=some"]);
    assert!(r.is_ok(), "{}", r.unwrap_err());
    let m = r.unwrap();
    assert!(m.contains_id("arg"));
    assert_eq!(
        m.get_one::<String>("arg").map(|v| v.as_str()).unwrap(),
        "default"
    );
}
```


### Steps to reproduce the bug with the above code

1. Add this to tests/builder/default_vals.rs in the clap repo.
2. `cargo test default_ifs_arg_required_not_present`

### Actual Behaviour

error: The following required arguments were not provided:
    <arg>

### Expected Behaviour

Should parse.

### Additional Context

This error started occuring sometime after 3.1.12

### Debug Output

_No response_

---

_Label `C-bug` added by @dsherret on 2022-06-15 17:13_

---

_Referenced in [denoland/deno#14876](../../denoland/deno/pulls/14876.md) on 2022-06-15 17:15_

---

_Renamed from "Using `default_value_ifs` with `required` returns error about argument not being provided sometime after 3.1.12" to "Using `default_value_ifs` with `required` returns error about argument not being provided when default is matched sometime after 3.1.12" by @dsherret on 2022-06-15 17:16_

---

_Comment by @epage on 2022-06-15 17:33_

That was broken with #3793 which was released in v3.2.0.

This is a tricky one.

This was not a documented or tested use case and in general our various validation rules should be for non-default arguments (we've been fine making similar changes across minor releases).  We were also needing to allow `default_value` and `required` to play nicely for the new actions like `ArgAction::SetTrue` which was needed by the derive API (see #3786).  At this point, a lot depends on this combination and can't be trivially backed out.

On the other hand, this broke you.

We'll need to do some investigation on this to see if there is a way to make both cases work.  Otherwise we'll either need to
- Say this is ok and leave it as-is
- Yank the 3.2 releases and release a 4.0 and push back our other changes to a 5.0.

---

_Label `E-hard` added by @epage on 2022-06-15 17:33_

---

_Label `A-validators` added by @epage on 2022-06-15 17:33_

---

_Comment by @JoelMon on 2022-06-16 01:27_

I think I'm having the same problem:

```rust
#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    /// The PO csv file to be used
    #[clap(short, long, parse(from_os_str), required_unless_present = "gui")]
    input: PathBuf,
    /// The destination directory where the processed POs will be saved
    #[clap(short, long, parse(from_os_str))]
    output: PathBuf,
    /// The text file that contains all of the store numbers to be processed
    #[clap(short, long, parse(from_os_str))]
    list: PathBuf,
    /// Print all RFIDs including items marked with a '$'
    #[clap(short = 'a', long = "print-all")]
    printall: bool,
    /// Produce a report of selected PO
    #[clap(short, long, conflicts_with_all = &["printall"])]
    report: bool,
    /// Runs LISA in GUI mode
    #[clap(long = "gui", exclusive = true)]
    gui: bool,
}
```
```
$ cargo run -- --gui
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/lisa --gui`
error: The following required argument was not provided: input

USAGE:
    lisa [OPTIONS] --input <INPUT> --output <OUTPUT> --list <LIST>

For more information try --help
```

```
$ cargo run -- --gui -i something
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/lisa --gui -i something`
error: The argument '--gui' cannot be used with one or more of the other specified arguments

USAGE:
    lisa [OPTIONS] --input <INPUT> --output <OUTPUT> --list <LIST>

For more information try --help
```

Is this the same problem or am I just making a mistake somewhere?

Full source code can be found here: [joelmon/lisa](https://github.com/JoelMon/LISA/tree/egui)
The above snippit can be found here: [main.rs:441](https://github.com/JoelMon/LISA/blob/egui/src/main.rs#L441)

---

_Comment by @epage on 2022-06-16 01:49_

I pinned this with clap 3.1 and it still produces the error.

```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! #clap = { path = "../clap", features = ["derive"] }
//! clap = { version = "=3.1", features = ["derive"] }
//! ```

use clap::{Args, Parser, Subcommand};
use std::path::PathBuf;

#[derive(Parser, Debug)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    /// The PO csv file to be used
    #[clap(short, long, parse(from_os_str), required_unless_present = "gui")]
    input: PathBuf,
    /// The destination directory where the processed POs will be saved
    #[clap(short, long, parse(from_os_str))]
    output: PathBuf,
    /// The text file that contains all of the store numbers to be processed
    #[clap(short, long, parse(from_os_str))]
    list: PathBuf,
    /// Print all RFIDs including items marked with a '$'
    #[clap(short = 'a', long = "print-all")]
    printall: bool,
    /// Produce a report of selected PO
    #[clap(short, long, conflicts_with_all = &["printall"])]
    report: bool,
    /// Runs LISA in GUI mode
    #[clap(long = "gui", exclusive = true)]
    gui: bool,
}

fn main() {
    let cli = Cli::parse();
    dbg!(cli);
}
```

I think what is going on is that clap sees `input` isn't of `Option<T>` and sets `required = true`.  If you change input to use `Option<T>`, it then gets past that error to complaining that `output` is required.

---

_Comment by @JoelMon on 2022-06-16 02:09_

> I think what is going on is that clap sees `input` isn't of `Option<T>` and sets `required = true`. If you change input to use `Option<T>`, it then gets past that error to complaining that `output` is required.

You are right. It works as expected once the types became of the type Option<T>. Thank you!

---

_Referenced in [clap-rs/clap#3936](../../clap-rs/clap/issues/3936.md) on 2022-07-16 02:29_

---

_Referenced in [containers/conmon-rs#719](../../containers/conmon-rs/pulls/719.md) on 2022-09-15 10:24_

---

_Referenced in [clap-rs/clap#4713](../../clap-rs/clap/issues/4713.md) on 2023-02-22 13:33_

---

_Comment by @epage on 2023-02-22 13:35_

Closing this as the new behavior is the expected behavior and the concern was with existing users which only means 3.x users.  At this point, making a change to something like this would be too disruptive to `v3-master` branch

---

_Closed by @epage on 2023-02-22 13:35_

---

_Comment by @crowlKats on 2023-08-13 01:50_

@epage Deno actually relied on this behaviour (`deno run --v8-flags=--help` would be allowed but any other value for the `v8-flags` flag would require also a file to be specified as a positional final argument), but only now figured out (and only recently even really noticed) why it hasn't worked properly since we upgraded clap past v3.2 (ref denoland/deno#20145).
Is there any interest in revisiting this behaviour or adding capability to enable it?

---

_Referenced in [denoland/deno#20145](../../denoland/deno/pulls/20145.md) on 2023-08-13 01:51_

---

_Comment by @epage on 2023-08-21 18:23_

Considering the existing behavior is the expected behavior, it would require v5 for us to break it.  I also don't see us likely to change course on this which would require new validation logic.  In general, I've been trying to hold off on new validation logic because there is a long tail of feature requests for it and would go counter to our build-time / binary size goals since it'd be all-or-nothing.  Instead, I've been holding out for #3476 which will allow us and users to have more validation options without impacting build-time and binary size as much.  Unfortunately, #3476 doesn't feel like enough of a priority atm for me to set aside the other stuff I'm doing to focus on it.

---

_Referenced in [clap-rs/clap#6031](../../clap-rs/clap/issues/6031.md) on 2025-06-10 19:29_

---
