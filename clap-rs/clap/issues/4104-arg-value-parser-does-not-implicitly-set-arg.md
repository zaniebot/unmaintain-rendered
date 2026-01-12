```yaml
number: 4104
title: "Arg::value_parser does not implicitly set Arg::takes_value"
type: issue
state: closed
author: wookietreiber
labels:
  - C-bug
assignees: []
created_at: 2022-08-23T07:14:42Z
updated_at: 2022-08-23T13:42:30Z
url: https://github.com/clap-rs/clap/issues/4104
synced_at: 2026-01-12T16:14:15Z
```

# Arg::value_parser does not implicitly set Arg::takes_value

---

_@wookietreiber_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.63.0 (4b91a6ea7 2022-08-08)

### Clap Version

3.2.17

### Minimal reproducible code

```rust
fn main() {
    let digest = clap::Arg::with_name("digest")
        .short('d')
        .long("digest")
        .help("choose digest")
        .value_parser(["md5", "sha1"]);

    let cli = clap::Command::new("cmd").arg(digest);

    dbg!(cli.get_matches());
}
```


### Steps to reproduce the bug with the above code

```console
$ cargo run -- --help
   Compiling clap-value-parser-takes-value v0.1.0 (/home/umcdev/src/bug-reproduction/clap-value-parser-takes-value)
    Finished dev [unoptimized + debuginfo] target(s) in 0.91s
     Running `target/debug/clap-value-parser-takes-value --help`
cmd

USAGE:
    clap-value-parser-takes-value [OPTIONS]

OPTIONS:
    -d, --digest    choose digest
    -h, --help      Print help information

$ cargo run -- --digest md5
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/clap-value-parser-takes-value --digest md5`
error: Found argument 'md5' which wasn't expected, or isn't valid in this context

USAGE:
    clap-value-parser-takes-value [OPTIONS]

For more information try --help
```

### Actual Behaviour

`Arg::value_parse` does not implicitly set `Arg::takes_value`.

### Expected Behaviour

A lot of functions, e.g. `Arg::value_name` set `Arg::takes_value(true)` that I expected `Arg::value_parser` to do the same. 

### Additional Context

I consider this to be an inconsistency, it does not follow the principle of least surprise. 

### Debug Output

not required.

---

_Label `C-bug` added by @wookietreiber on 2022-08-23 07:14_

---

_Comment by @epage on 2022-08-23 13:42_

It might not be obvious at first but `value_parser` applies to flags as well as option and arguments.

The first case is defining how to parse environment variables.

The other is for [non-bool flags](https://github.com/clap-rs/clap/pull/4092/files)

---

_Closed by @epage on 2022-08-23 13:42_

---
