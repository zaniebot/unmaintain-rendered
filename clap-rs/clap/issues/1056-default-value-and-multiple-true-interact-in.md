---
number: 1056
title: default_value() and multiple(true) interact in surprising ways.
type: issue
state: closed
author: Screwtapello
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2017-09-28T07:29:42Z
updated_at: 2018-08-02T03:30:12Z
url: https://github.com/clap-rs/clap/issues/1056
synced_at: 2026-01-10T01:26:42Z
---

# default_value() and multiple(true) interact in surprising ways.

---

_Issue opened by @Screwtapello on 2017-09-28 07:29_

### Rust Version

rustc 1.20.0 (f3d6973f4 2017-08-27)

### Affected Version of clap

clap 2.26.2

### Steps to Reproduce the issue

I want to write a program that's a little bit like `ssh`: it does some setup, then runs the rest of the command-line arguments as a new process. If there are no other arguments, it just runs a shell. Looking at the clap documentation, I came up with the following pattern:

```rust
extern crate clap;

fn main() {
    let matches = clap::App::new("claptest")
        .setting(clap::AppSettings::TrailingVarArg)
        .arg(clap::Arg::with_name("command")
            .multiple(true)
            .default_value("/bin/sh")
        )
        .get_matches();

    let command: Vec<_> = matches.values_of_os("command").unwrap().collect();

    println!("Command to run: {:?}", command);
}                                                                               
```

If I build this and run it with no arguments, I get the default, just as I wanted:

```
$ ./target/debug/claptest 
Command to run: ["/bin/sh"]
```

If I give it a different command to run, I get the default *appended* to the command I gave:

```
$ ./target/debug/claptest foo bar
Command to run: ["foo", "bar", "/bin/sh"]
```

If there's an explicit value on the command-line, I'd expect that to *replace* the default, not *extend* it:

```
$ ./target/debug/claptest foo bar
Command to run: ["foo", "bar"]
```

### Workarounds

If I don't use `.default_value()`, I can manually apply the default later, but of course then it doesn't show up in the `--help` output with all the other defaults.

`.occurrences_of()` returns 0 if only the default is present, 1 if there's one explicit argument, etc. I could do something like:

```rust
let command: Vec<_> = if matches.occurrences_of("command") > 0 {
    matches.values_of_os("command").unwrap().take(matches.occurrences_of("command"))
} else {
    matches.values_of_os("command").unwrap()
}.collect();
```

...but man, that's a mouthful.

---

_Comment by @kbknapp on 2017-10-04 03:08_

This is a bug, thanks for reporting!

I'd be willing to mentor someone to fix this, which should be a super quick fix if someone wants an easy PR. Otherwise I can try and knock it out this week once I get a little caught up. 

If anyone is interested, the code to potentially fix this issue [is here](https://github.com/kbknapp/clap-rs/blob/3224e2e1cd4927935f44081c1ca686d95f267790/src/app/parser.rs#L1728-L1790). You'd simply need to ensure there are no values before adding the defaults.

---

_Label `C: parsing` added by @kbknapp on 2017-10-04 03:08_

---

_Label `D: easy` added by @kbknapp on 2017-10-04 03:08_

---

_Label `P2: need to have` added by @kbknapp on 2017-10-04 03:08_

---

_Label `T: bug` added by @kbknapp on 2017-10-04 03:08_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-04 03:08_

---

_Referenced in [clap-rs/clap#1050](../../clap-rs/clap/issues/1050.md) on 2017-10-04 03:14_

---

_Comment by @kbknapp on 2017-10-04 03:15_

Duplicate of #1050 

---

_Added to milestone `2.27.0` by @kbknapp on 2017-10-12 21:36_

---

_Referenced in [steveklabnik/rustdoc#197](../../steveklabnik/rustdoc/pulls/197.md) on 2017-10-20 21:14_

---

_Closed by @kbknapp on 2017-10-25 01:49_

---
