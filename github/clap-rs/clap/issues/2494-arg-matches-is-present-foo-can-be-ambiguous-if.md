---
number: 2494
title: "arg_matches.is_present(\"foo\") can be ambiguous if there are an arg and a subcommand named foo"
type: issue
state: closed
author: felipesere
labels:
  - C-bug
assignees: []
created_at: 2021-05-20T17:54:12Z
updated_at: 2021-05-25T23:40:49Z
url: https://github.com/clap-rs/clap/issues/2494
synced_at: 2026-01-07T13:12:19-06:00
---

# arg_matches.is_present("foo") can be ambiguous if there are an arg and a subcommand named foo

---

_Issue opened by @felipesere on 2021-05-20 17:54_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.51.0 (2fd73fabe 2021-03-23)

### Clap Version

master

### Minimal reproducible code

```rust
#[test]
fn global_args_are_weird_felipe() {
    let app = App::new("opt")
        .arg(Arg::new("global").global(true).long("global"))
        .subcommand(App::new("global"));

    let m = app.clone().get_matches_from(&["opt", "global", "--global"]);
    assert!(m.is_present("global")); // flag and subcommand are present...
    assert!(m.subcommand_matches("global").unwrap().is_present("global"));

    let m = app.clone().get_matches_from(&["opt", "--global"]);
    assert!(m.is_present("global")); // this should be the flag right now
    assert!(m.subcommand_matches("global").is_none());

    let m = app.get_matches_from(&["opt", "global"]);
    assert!(!m.subcommand_matches("global").unwrap().is_present("global"));
    assert!(!m.is_present("global")); // if this is the flag, it should be false
}

```


### Steps to reproduce the bug with the above code

The test above passes w

### Actual Behaviour

`arg_matches.is_present("foo")` can't tell the difference between "foo" as an argument or "foo" as subcommand being present.

### Expected Behaviour

Possibly, there should be two methods
```
is_arg_present("...")
// and
is_subcommand_present("...")
```
That way we can consistently check for the right presence.

### Additional Context

This surfaced when working on `from_global`.

### Debug Output

_No response_

---

_Label `T: bug` added by @felipesere on 2021-05-20 17:54_

---

_Referenced in [clap-rs/clap#2026](../../clap-rs/clap/pulls/2026.md) on 2021-05-20 17:54_

---

_Label `C: matches` added by @pksunkara on 2021-05-20 18:20_

---

_Added to milestone `3.0` by @pksunkara on 2021-05-20 18:20_

---

_Closed by @pksunkara on 2021-05-25 23:40_

---
