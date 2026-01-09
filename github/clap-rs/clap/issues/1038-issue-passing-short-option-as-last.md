---
number: 1038
title: Issue passing short option as last 
type: issue
state: closed
author: snowe2010
labels: []
assignees: []
created_at: 2017-08-25T13:46:36Z
updated_at: 2018-08-02T03:30:11Z
url: https://github.com/clap-rs/clap/issues/1038
synced_at: 2026-01-07T13:12:19-06:00
---

# Issue passing short option as last 

---

_Issue opened by @snowe2010 on 2017-08-25 13:46_

### Rust Version

`rustc 1.21.0-nightly (469a6f9bd 2017-08-22)`

### Affected Version of clap

```
> grep clap Cargo.lock 
 "clap 2.26.0 (registry+https://github.com/rust-lang/crates.io-index)",
name = "clap"
"checksum clap 2.26.0 (registry+https://github.com/rust-lang/crates.io-index)" = "2267a8fdd4dce6956ba6649e130f62fb279026e5e84b92aa939ac8f85ce3f9f0"
```

but this was also happening on 2.21 as I tested on that before upgrading

### Expected Behavior Summary
```
cargo run start -- -g -i -s --blah
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/pt start -g -i -s --blah`
works here....✔
```

### Actual Behavior Summary

```
   Compiling clap v2.26.0
   Compiling pt v0.2.2 (file:///Users/tylerthrailkill/Dropbox/rustup/cli)
    Finished dev [unoptimized + debuginfo] target(s) in 31.36 secs
     Running `target/debug/pt start -g -i -s -b`
error: Found argument '-b' which wasn't expected, or isn't valid in this context

USAGE:
    pt start --flag1 <BRANCH> --flag2 <BRANCH> --flag3 <BRANCH>

For more information try --help
```

### Steps to Reproduce the issue
pass multiple short options

### Sample Code or Link to Sample Code

#### examples/test.rs
```
#[macro_use]
extern crate clap;

#[cfg(feature = "yaml")]
fn main() {
    use clap::App;

    let yml = load_yaml!("test.yml");
    let m = App::from_yaml(yml).get_matches();

    match m.subcommand() {
        ("subcmd", Some(matches)) => println!("did it"),
	_ => panic!("couldn't find that subcommand")
    }
    
    if let Some(mode) = m.value_of("flag") {
        println!("here");
    } else {
        println!("--flag <BRANCH> wasn't used...");
    }
}

#[cfg(not(feature = "yaml"))]
fn main() {
    println!("YAML feature is disabled.");
    println!("Pass --features yaml to cargo when trying this example.");
}
```

#### examples/test.yml
```
name: Example showing failure
version: "0.0.1"
author: Tyler B. Thrailkill <tyler.b.thrailkill@gmail.com>
about: Example showing failure
subcommands:
  - start:
      about: "example showing failure"
      version: "0.0.1"
      author: Tyler B. Thrailkill <tyler.b.thrailkill@gmail.com>
      args:
        - flag1:
            short: s
            long: flag1
            takes_value: true
            value_name: BRANCH
            min_values: 0
            max_values: 1
            default_value: develop
            help: |
              runs flag1 with an optional branch
        - flag2:
            short: g
            long: flag2
            takes_value: true
            value_name: BRANCH
            max_values: 1
            min_values: 0
            default_value: develop
            help: |
              runs flag2 with an optional branch
        - flag3:
            short: i
            long: flag3
            takes_value: true
            value_name: BRANCH
            max_values: 1
            min_values: 0
            default_value: develop
            help: |
              runs flag3 with an optional branch
        - flag4:
            short: w
            long: flag4
            takes_value: true
            value_name: BRANCH
            max_values: 1
            min_values: 0
            default_value: develop
            help: |
              runs flag4 with an optional branch
```

### Debug output

https://gist.github.com/snowe2010/71b09a435d24533a69a5337b1a150f91


---

_Comment by @kbknapp on 2017-08-26 20:27_

I'm not seeing where you defined `-b` anywhere? Are you saying you'd like to be able to use `-b` as a value? If so, try `Arg::allow_hyphen_values(true)`. 

---

_Label `T: RFC / question` added by @kbknapp on 2017-08-26 20:29_

---

_Comment by @snowe2010 on 2017-08-27 21:50_

![facepalm](https://media.giphy.com/media/gnJgBlPgHtcnS/giphy.gif)

---

_Closed by @snowe2010 on 2017-08-27 21:50_

---
