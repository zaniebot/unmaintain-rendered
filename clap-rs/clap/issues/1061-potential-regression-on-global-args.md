```yaml
number: 1061
title: Potential regression on global args
type: issue
state: closed
author: Freyskeyd
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2017-10-07T08:32:38Z
updated_at: 2018-08-02T03:30:12Z
url: https://github.com/clap-rs/clap/issues/1061
synced_at: 2026-01-12T16:14:10Z
```

# Potential regression on global args

---

_@Freyskeyd_

### Rust Version

`rustc 1.22.0-nightly (417c73891 2017-10-05)`

### Affected Version of clap

`clap 2.26.2`

### Expected Behavior Summary

Using a YAML definition with an `arg` named `quiet`, defined as `global` I'm expecting to be able to use it in any subcommands.

### Actual Behavior Summary

The `quiet` flag are only usable on default command, but disapear in subcommands.

### Steps to Reproduce the issue

Here's the `yaml`
```yaml
name: Test cli
version: "1.0"
args:
    - quiet:
        short: q
        help: Display only needed output
        global: true
subcommands:
    - auth:
        about: Authenticate command
```

`main.rs`
```rust
fn main() {
    let yaml = load_yaml!("cli.yml");
    let app = App::from_yaml(yaml);

    let matches = app.get_matches();

    println!("quiet: {:?}", matches.is_present("quiet"));
    println!("quiet_o: {:?}", matches.occurrences_of("quiet"));
}
```

### Debug output
```bash
$ ./target/debug/cli
quiet: false
quiet_o: 0

$ ./target/debug/cli -q
quiet: true
quiet_o: 1

$ ./target/debug/cli -q auth
quiet: true
quiet_o: 1

$ ./target/debug/cli auth -q
quiet: false
quiet_o: 0
```

---

_Comment by @kbknapp on 2017-10-07 17:03_

Thanks for filing this! I'll add it to the list as it might be related to #978  and the associated PR #1060 

---

_Label `C: args` added by @kbknapp on 2017-10-07 17:03_

---

_Label `C: parsing` added by @kbknapp on 2017-10-07 17:03_

---

_Label `C: subcommands` added by @kbknapp on 2017-10-07 17:03_

---

_Label `D: intermediate` added by @kbknapp on 2017-10-07 17:03_

---

_Label `P2: need to have` added by @kbknapp on 2017-10-07 17:03_

---

_Label `Regression` added by @kbknapp on 2017-10-07 17:03_

---

_Label `T: bug` added by @kbknapp on 2017-10-07 17:03_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-07 17:03_

---

_Added to milestone `2.27.0` by @kbknapp on 2017-10-12 21:35_

---

_Comment by @kbknapp on 2017-10-18 14:44_

@Freyskeyd could you test this with the current master branch and see if it's fixed? #1070 just merged which may have helped. If not I can look into it.

---

_Comment by @Freyskeyd on 2017-10-19 07:37_

@kbknapp I will try it as soon as I can :)

---

_Comment by @Freyskeyd on 2017-10-20 08:03_

@kbknapp Unfortunately, it's doesn't work with 569ced1f

---

_Referenced in [clap-rs/clap#1072](../../clap-rs/clap/pulls/1072.md) on 2017-10-20 10:34_

---

_Closed by @kbknapp on 2017-10-24 12:01_

---
