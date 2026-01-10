---
number: 2445
title: Trailing newlines are missing for long_help output when term width is exceeded  
type: issue
state: closed
author: nkaretnikov
labels:
  - C-bug
assignees: []
created_at: 2021-04-18T00:38:11Z
updated_at: 2021-04-18T12:33:50Z
url: https://github.com/clap-rs/clap/issues/2445
synced_at: 2026-01-10T01:27:17Z
---

# Trailing newlines are missing for long_help output when term width is exceeded  

---

_Issue opened by @nkaretnikov on 2021-04-18 00:38_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.49.0-nightly (ffa2e7ae8 2020-10-24)

### Clap Version

2.33.3

### Minimal reproducible code

```rust
use clap;

fn main() {
    let _args = clap::App::new("test")
        .set_term_width(80)

        .arg(clap::Arg::with_name("foo")
            .short("f")
            .long("foo")
            .takes_value(true)
            .value_name("FOO")
            .required(false)
            .long_help("This is a very very very very very very very very very very very long help message"))

        .arg(clap::Arg::with_name("bar")
            .short("b")
            .long("bar")
            .takes_value(true)
            .value_name("BAR")
            .required(false)
            .long_help("This is a very very very very very very very very very very very long help message"))

        .arg(clap::Arg::with_name("baz")
            .short("z")
            .long("baz")
            .takes_value(true)
            .value_name("BAZ")
            .required(false)
            .long_help("This is a short message"))

        .arg(clap::Arg::with_name("qux")
            .short("q")
            .long("qux")
            .takes_value(true)
            .value_name("QUX")
            .required(false)
            .long_help("This is a very very very very very very very very very very very long help message"))

        .arg(clap::Arg::with_name("quux")
            .short("u")
            .long("quux")
            .takes_value(true)
            .value_name("QUUX")
            .required(false)
            .long_help("This is a very very very very very very very very very very very long help message"))

        .get_matches();
}
```


### Steps to reproduce the bug with the above code

cargo run -- --help

### Actual Behaviour

Trailing newlines are missing for options that were wrapped around.

```
test

USAGE:
    test-rs [OPTIONS]

FLAGS:
    -h, --help
            Prints help information

    -V, --version
            Prints version information


OPTIONS:
    -b, --bar <BAR>
            This is a very very very very very very very very very very very
            long help message
    -z, --baz <BAZ>
            This is a short message

    -f, --foo <FOO>
            This is a very very very very very very very very very very very
            long help message
    -u, --quux <QUUX>
            This is a very very very very very very very very very very very
            long help message
    -q, --qux <QUX>
            This is a very very very very very very very very very very very
```

### Expected Behaviour

Trailing newlines are present for options that were wrapped around.

```
test

USAGE:
    test-rs [OPTIONS]

FLAGS:
    -h, --help
            Prints help information

    -V, --version
            Prints version information


OPTIONS:
    -b, --bar <BAR>
            This is a very very very very very very very very very very very
            long help message

    -z, --baz <BAZ>
            This is a short message

    -f, --foo <FOO>
            This is a very very very very very very very very very very very
            long help message

    -u, --quux <QUUX>
            This is a very very very very very very very very very very very
            long help message

    -q, --qux <QUX>
            This is a very very very very very very very very very very very
```

### Additional Context

Using `long_help` instead of `help` because `help` also results in inconsistent formatting. This is slightly better, but the lack of trailing newlines is still a problem.

### Debug Output

_No response_

---

_Label `T: bug` added by @nkaretnikov on 2021-04-18 00:38_

---

_Comment by @pksunkara on 2021-04-18 01:02_

Should be fixed in master.

---

_Closed by @pksunkara on 2021-04-18 01:02_

---

_Comment by @nkaretnikov on 2021-04-18 12:33_

@pksunkara tested on d253c34, seems to work (after changing some things to match the new API). thx for the quick response! 

---
