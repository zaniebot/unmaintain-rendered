---
number: 1268
title: AllowLeadingHyphen and AllowNegativeNumbers breaks argument parsing for subsequent arguments
type: issue
state: closed
author: Jarngreipr
labels: []
assignees: []
created_at: 2018-05-04T10:43:17Z
updated_at: 2018-08-02T03:30:23Z
url: https://github.com/clap-rs/clap/issues/1268
synced_at: 2026-01-10T01:26:47Z
---

# AllowLeadingHyphen and AllowNegativeNumbers breaks argument parsing for subsequent arguments

---

_Issue opened by @Jarngreipr on 2018-05-04 10:43_

### Rust Version

rustc 1.27.0-nightly (7f3444e1b 2018-04-26)

### Affected Version of clap

clap 2.31.2 (registry+https://github.com/rust-lang/crates.io-index)"

### Expected Behavior Summary

Enabling AppSettings::{AllowLeadingHyphen, AllowNegativeNumbers} and argument setting `allow_hyphen_values` should allow optional arguments to contain a hyphen without affecting subsequently passed arguments:

```
myapp --hyphenated "-20" --other
```

### Actual Behavior Summary

Testing revealed that arguments passed after an argument that takes a hyphen value are not parsed.
If there are required arguments among them, an error stating that they were not passed.
Other arguments are silently unparsed.

### Steps to Reproduce the issue

Enable any of `AllowLeadingHyphen`, `AllowNegativeNumbers`, `allow_hyphen_value`.
Pass an argument that takes a hyphenated value, then pass another argument (preferably a required one).

### Sample Code or Link to Sample Code

```
#[test]
fn test_negative_builder() {
    let m = App::new("minus")
            .subcommand(SubCommand::with_name("num")
            .about("Plots data from given simulation paths")
            .args(&[
                    Arg::with_name("neg")
                           .short("n")
                           .takes_value(true)
                           .allow_hyphen_values(true)
                           .multiple(true),
                    Arg::with_name("req")
                           .short("r")
                           .required(true),
            ])).get_matches_from(vec!["minus", "num","-n", "-0.5,0.5", "-r"]);
    if let Some(matches) = m.subcommand_matches("num") {
        let val: Vec<&str> = matches.values_of("neg").unwrap().collect();
        let vals: Vec<&str> = val[0].split(",").collect();
        assert_eq!(vals, vec!["-0.5", "0.5"]);
    }   
}
```

The same test with `.get_matches_from(vec!["minus", "num", "-r","-n", "-0.5,0.5"])` should pass.

### Debug output
N/A


---

_Comment by @scirner22 on 2018-05-18 04:09_

Try attaching `AllowLeadingHyphen` to the subcommand and not the app.

---

_Comment by @Jarngreipr on 2018-06-04 11:27_

@scirner22 Your suggestion does not appear to resolve the issue.
Adding `.setting(AppSettings::AllowLeadingHyphen)` after `.subcommand(SubCommand::with_name("num")`
and using `.get_matches_from(vec!["minus", "num","-n", "-0.5,0.5", "-r"])` fails with the following:
```
error: The following required arguments were not provided:
    -r
```
What appears to be happening is that after passing `-n "-0.5"`, all subsequent arguments will be interpreted as values instead of arguments, thus the required argument `-r` is not found which causes the error above.

---

_Comment by @kbknapp on 2018-06-05 02:08_

This is why the docs suggest using `Arg::allow_hyphen_values` instead of an application wide `AppSettings::AllowLeadingHyphen`. If you *must* use an application wide setting, it's very much recommended to use things like `Arg::number_of_values(1)` for your arguments to limit the number of values they could use in a row.

Otherwise, clap has no way of knowing when to stop parsing values. If all you say is, "all values can start with a `-`, *and* they accept multiple values" clap really can't tell when one argument stops and another one starts. This may sound simple because, "if it matches an argument I defined of course it's an argument and not a value!" Unfortunately, it's not that simple because there are many times when a value may also equal an argument you defined, especially when working with shorts and negative numbers.

I'm going to close this as the best thing for a CLI UX perspective is to limit the number of values in a row, or design the CLI so that such arguments which accept negative numbers or hyphen values always come last (such as with `Arg::last` or `AppSettings::TrailingVarArg`). If you have some other specifics on your use case and why you may or may not be able to use these limitations, please ping me or hit me up in the Gitter channel and we can discuss further :wink:

---

_Closed by @kbknapp on 2018-06-05 02:08_

---
