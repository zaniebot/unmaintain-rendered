```yaml
number: 3059
title: requires_ifs does not appear to work
type: issue
state: closed
author: FrankC01
labels:
  - C-bug
assignees: []
created_at: 2021-12-04T11:16:26Z
updated_at: 2021-12-10T22:16:57Z
url: https://github.com/clap-rs/clap/issues/3059
synced_at: 2026-01-12T16:14:14Z
```

# requires_ifs does not appear to work

---

_@FrankC01_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

clap = "2.33.3"

### Minimal reproducible code

```rust
    #[test]
    fn test_output() {
        let res = App::new("prog")
            .arg(
                Arg::with_name("output")
                    .long("output")
                    .short("o")
                    .takes_value(true)
                    .possible_values(&["csv", "excel", "stdout"])
                    .default_value("stdout")
                    .help("Direct output to file"),
            )
            .arg(
                Arg::with_name("filename")
                    .long("filename")
                    .short("f")
                    .takes_value(true)
                    // .requires_if("excel", "output") 
                    // .requires_ifs(&[("output", "excel"), ("output", "csv")])
                    .requires_ifs(&[("excel", "output"), ("csv", "output")])
                    .help("Filename for '-o excel' or '-o csv' output"),
            )
            .get_matches_from_safe(vec!["prog", "-o", "excel"]);
        println!("{:?}", res);
        // assert!(res.is_err()); // We  used -o excel so -f <filename> is required
        // assert_eq!(res.unwrap_err().kind, ErrorKind::MissingRequiredArgument);
    }
```


### Steps to reproduce the bug with the above code

cargo test

### Actual Behaviour

No error occurred

### Expected Behaviour

Expected error as the `-f <val>` is required if `-o excel` or `-o csv` is present

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @FrankC01 on 2021-12-04 11:16_

---

_Comment by @FrankC01 on 2021-12-04 11:45_

This works if I change

` .requires_ifs(&[("output", "excel"), ("output", "csv")])`

to

` .required_ifs(&[("output", "excel"), ("output", "csv")])`

---

_Comment by @epage on 2021-12-04 17:53_

Yes
- a "requires" relationship says `filename` cannot be present unless X conditions are met (`prog -f file` will fail without `-o excel`)
- a "required" relationship says `filename` must be present if X conditions are met (`prog -o excel` will fail without `-f file`)

---

_Comment by @epage on 2021-12-04 17:54_

And I do feel like we could do a better job clarifying that distinction

---

_Comment by @FrankC01 on 2021-12-05 09:09_

However, while `required_ifs` fails if `-f <arg>` is not supplied with `-o <arg>`, it **_does not_** fail if I do `prog -f somefile` 

Basically I'm looking for a way to:
1. If `-f <arg>` appears without `-o <arg` **then fail**
2. if `-o <arg>` appears without `-f <arg>` **then fail**

Am I missing some magic combo?

---

_Comment by @epage on 2021-12-06 13:00_

Could you provide code with those cases so we know which APIs you are using in which ways to try to get the behavior you are wanting that isn't working as expected?

---

_Comment by @FrankC01 on 2021-12-06 17:41_

Basically I want the two options to be required of each other `-o <arg> -f<arg>`
I will go through iterations of what I've tried and add them here

---

_Referenced in [epage/clapng#246](../../epage/clapng/issues/246.md) on 2021-12-06 23:14_

---

_Comment by @FrankC01 on 2021-12-08 21:56_

@epage Here are the four combo's (2-required_ifs and 2-requires_ifs). I expect all to fail but only the first does:
```rust
    #[test]
    fn test_requiredifs_options_without_file_fail() {
        let res = App::new("prog")
            .arg(
                Arg::with_name("output")
                    .long("output")
                    .short("o")
                    .takes_value(true)
                    .possible_values(&["csv", "excel", "stdout"])
                    .default_value("stdout")
                    .help("Direct output to file"),
            )
            .arg(
                Arg::with_name("filename")
                    .long("filename")
                    .short("f")
                    .takes_value(true)
                    .required_ifs(&[("output", "excel"), ("output", "csv")])
                    .help("Filename for '-o excel' or '-o csv' output"),
            )
            .get_matches_from_safe(vec!["prog", "-o", "excel"]);
        assert!(res.is_err()); // We  used -o excel so -f <filename> is required
        assert_eq!(res.unwrap_err().kind, ErrorKind::MissingRequiredArgument);
    }
    #[test]
    fn test_requiredifs_options_without_output_should_fail() {
        let res = App::new("prog")
            .arg(
                Arg::with_name("output")
                    .long("output")
                    .short("o")
                    .takes_value(true)
                    .possible_values(&["csv", "excel", "stdout"])
                    .default_value("stdout")
                    .help("Direct output to file"),
            )
            .arg(
                Arg::with_name("filename")
                    .long("filename")
                    .short("f")
                    .takes_value(true)
                    .required_ifs(&[("output", "excel"), ("output", "csv")])
                    .help("Filename for '-o excel' or '-o csv' output"),
            )
            .get_matches_from_safe(vec!["prog", "-f", "filename"]);
        assert!(res.is_err()); // We  used -o excel so -f <filename> is required
        assert_eq!(res.unwrap_err().kind, ErrorKind::MissingRequiredArgument);
    }
    #[test]
    fn test_requiresif_options_without_file_should_fail() {
        let res = App::new("prog")
            .arg(
                Arg::with_name("output")
                    .long("output")
                    .short("o")
                    .takes_value(true)
                    .possible_values(&["csv", "excel", "stdout"])
                    .default_value("stdout")
                    .help("Direct output to file"),
            )
            .arg(
                Arg::with_name("filename")
                    .long("filename")
                    .short("f")
                    .takes_value(true)
                    .requires_ifs(&[("output", "excel"), ("output", "csv")])
                    .help("Filename for '-o excel' or '-o csv' output"),
            )
            .get_matches_from_safe(vec!["prog", "-o", "excel"]);
        assert!(res.is_err()); // We  used -o excel so -f <filename> is required
        assert_eq!(res.unwrap_err().kind, ErrorKind::MissingRequiredArgument);
    }
    #[test]
    fn test_requiresif_options_without_output_should_fail() {
        let res = App::new("prog")
            .arg(
                Arg::with_name("output")
                    .long("output")
                    .short("o")
                    .takes_value(true)
                    .possible_values(&["csv", "excel", "stdout"])
                    .default_value("stdout")
                    .help("Direct output to file"),
            )
            .arg(
                Arg::with_name("filename")
                    .long("filename")
                    .short("f")
                    .takes_value(true)
                    .requires_ifs(&[("output", "excel"), ("output", "csv")])
                    .help("Filename for '-o excel' or '-o csv' output"),
            )
            .get_matches_from_safe(vec!["prog", "-f", "somefile"]);
        assert!(res.is_err()); // We  used -o excel so -f <filename> is required
        assert_eq!(res.unwrap_err().kind, ErrorKind::MissingRequiredArgument);
    }
```

---

_Referenced in [clap-rs/clap#3145](../../clap-rs/clap/issues/3145.md) on 2021-12-10 20:25_

---

_Comment by @epage on 2021-12-10 20:38_

What you are looking for is something like
```rust
use clap::{App, Arg, ErrorKind};

fn main() {}

#[test]
fn test_requiresif_options_without_output_should_fail() {
    let mut app = App::new("prog")
        .arg(
            Arg::new("output")
                .long("output")
                .short('o')
                .takes_value(true)
                .possible_values(&["csv", "excel", "stdout"])
                .requires_ifs(&[("excel", "filename"), ("csv", "filename")])
                .help("Direct output to file"),
        )
        .arg(
            Arg::new("filename")
                .long("filename")
                .short('f')
                .takes_value(true)
                .requires("output")
                .help("Filename for '-o excel' or '-o csv' output"),
        );

    let res = app.try_get_matches_from_mut(vec!["prog", "-f", "somefile"]);
    assert!(res.is_err()); // We  used -o excel so -f <filename> is required
    assert_eq!(res.unwrap_err().kind, ErrorKind::MissingRequiredArgument);

    let res = app.try_get_matches_from_mut(vec!["prog", "-o", "excel"]);
    assert!(res.is_err()); // We  used -o excel so -f <filename> is required
    assert_eq!(res.unwrap_err().kind, ErrorKind::MissingRequiredArgument);

    let res = app.try_get_matches_from_mut(vec!["prog", "-o", "stdout"]);
    assert!(res.is_ok(), "{:?}", res);

    let res = app.try_get_matches_from_mut(vec!["prog", "-o", "excel", "-f", "file.xls"]);
    assert!(res.is_ok(), "{:?}", res);
}
```
This is written with clap3.  I've not checked to see if there are any gotchas backporting to clap2.

Even with clap3, I had to remove the `default_value`.  That can be added back in when we fix #3076 

---

_Comment by @FrankC01 on 2021-12-10 22:02_

Ok, so I had it bass-ackwards to begin with. I can work with your solution if you want to close this

And thanks for your help!

---

_Closed by @epage on 2021-12-10 22:16_

---
