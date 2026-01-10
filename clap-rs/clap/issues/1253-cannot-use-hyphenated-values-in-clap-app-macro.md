---
number: 1253
title: "Cannot use hyphenated values in clap_app! macro"
type: issue
state: closed
author: mattgathu
labels: []
assignees: []
created_at: 2018-04-18T19:09:44Z
updated_at: 2018-08-02T03:30:22Z
url: https://github.com/clap-rs/clap/issues/1253
synced_at: 2026-01-10T01:26:47Z
---

# Cannot use hyphenated values in clap_app! macro

---

_Issue opened by @mattgathu on 2018-04-18 19:09_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.27.0-nightly

### Affected Version of clap
2.31.2

### Expected Behavior Summary

I'm converting an argument to use the `clap_app!` macro

from
```rust
Arg::with_name("FILE")
        .short("O")
        .long("output-document")
        .help("write documents to FILE")
        .required(false)
        .takes_value(true)
```
to
```rust
 (@arg FILE: -O --output-document +takes_value "write documents to FILE")
```

Expected help output should be:

```
OPTIONS:
    -O, --output-document <FILE>    write documents to FILE
```

### Actual Behavior Summary

Instead the help output is

```
OPTIONS:
    -d, --output <FILE>        write documents to FILE
```


### Steps to Reproduce the issue

Use a hyphenated value for `long` when defining an argument using the `clap_app!` macro.

### Sample Code or Link to Sample Code

NA

### Debug output
NA


---

_Comment by @kbknapp on 2018-06-05 01:35_

Sorry for the long wait, I've been away with my day job.

You have to use `--("my-arg-name")` when using the macros. It's a limitation of the macro system in Rust :disappointed: 

---

_Closed by @kbknapp on 2018-06-05 01:35_

---

_Comment by @mattgathu on 2018-06-05 07:49_

@kbknapp is this documented? If not, maybe document this somewhere.

---

_Comment by @kbknapp on 2018-06-05 13:37_

@mattgathu [it is documented](https://docs.rs/clap/2.31.2/clap/macro.clap_app.html#shorthand-syntax-for-args) although I'll be the first to admit it's somewhat hidden :stuck_out_tongue_winking_eye: Unfortunately, macros in general are somewhat hard to document in Rust.

---

_Referenced in [clap-rs/clap#1248](../../clap-rs/clap/issues/1248.md) on 2018-06-05 13:41_

---
