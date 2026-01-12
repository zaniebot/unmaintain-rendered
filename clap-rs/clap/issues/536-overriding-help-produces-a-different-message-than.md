```yaml
number: 536
title: Overriding --help produces a different message than the default
type: issue
state: closed
author: brianp
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2016-06-23T16:51:24Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/536
synced_at: 2026-01-12T16:14:09Z
```

# Overriding --help produces a different message than the default

---

_@brianp_

When overriding the `help` flag using `long("help")`, the auto-generated help output is different from the default.

Differences include:
- The app `bin_name` in the usage example
- No `version` flag is displayed

---

**Example app, with expected default output:**

``` rust
fn main() {
    let app = App::new("test").get_matches();
}
```

Output:

``` bash
$ cargo run -- --help
     Running `target/debug/clapapp --help`
test

USAGE:
    clapapp [FLAGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

---

**Example app overriding help, and different output:**

``` rust
fn main() {
    let app = App::new("test")
                  .arg(Arg::with_name("help")
                       .short("h")
                       .long("help")
                       .help("Prints help information")
                       .takes_value(false));

    let matches = &app.clone().get_matches();

    if matches.is_present("help") {
        let _ = &app.print_help();
        println!("\n");
    };
}
```

Output:

``` bash
$ cargo run -- --help
     Running `target/debug/clapapp --help`
test

USAGE:
    test [FLAGS]

FLAGS:
    -h, --help    Prints help information
```


---

_Added to milestone `2.7.0` by @kbknapp on 2016-06-23 18:44_

---

_Label `T: bug` added by @kbknapp on 2016-06-23 18:44_

---

_Label `P2: need to have` added by @kbknapp on 2016-06-23 18:44_

---

_Label `C: help message` added by @kbknapp on 2016-06-23 18:44_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-23 18:44_

---

_Comment by @kbknapp on 2016-06-23 18:54_

I'm on mobile so I can't give a good link at the moment, but if you look at line 460 of `src/app/parser.rs` you'll see a call to `build_help_and_version` which never gets called when `print_help` is called.

Simply inserting that call will fix it, but also have the side effect that it's called twice on a normal run. So we either need to find a way to not call it twice, minimize the runtime impact if it is called twice, or something to the like.

Something that might help is use the `Parser::is_set` methods to check the internal AppSettings for things like `NeedsLongHelp` (or Version) instead of iterating all flags.


---

_Referenced in [clap-rs/clap#533](../../clap-rs/clap/issues/533.md) on 2016-06-24 00:19_

---

_Closed by @homu on 2016-06-24 05:00_

---
