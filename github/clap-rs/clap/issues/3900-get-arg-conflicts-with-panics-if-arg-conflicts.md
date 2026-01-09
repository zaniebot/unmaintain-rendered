---
number: 3900
title: get_arg_conflicts_with panics if arg conflicts with group.
type: issue
state: closed
author: tmccombs
labels:
  - C-bug
assignees: []
created_at: 2022-07-07T07:45:04Z
updated_at: 2022-07-14T14:39:22Z
url: https://github.com/clap-rs/clap/issues/3900
synced_at: 2026-01-07T13:12:20-06:00
---

# get_arg_conflicts_with panics if arg conflicts with group.

---

_Issue opened by @tmccombs on 2022-07-07 07:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

3.2.6

### Minimal reproducible code

```rust
use clap::*;

fn main() {
    let c = Arg::new("c").short('c').conflicts_with("gr");
    let app = Command::new("test")
        .group(ArgGroup::new("gr").args(&["a", "b"]))
        .arg(Arg::new("a").short('a'))
        .arg(Arg::new("b").short('b'))
        .arg(&c);
        
    let res =app.clone().try_get_matches_from(vec!["cmd", "-c", "-a"]);
    if let Err(e) = res {
        assert_eq!(e.kind(), clap::error::ErrorKind::ArgumentConflict);
        e.print().unwrap();
    } else {
        unreachable!();
    }

    println!("conflicts: {:?}", app.get_arg_conflicts_with(&c));
}
```

(see https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=6db4239226ac59fd07ccc37d0ce13bdd)


### Steps to reproduce the bug with the above code

Run the code in the minimal reproducible code section above

### Actual Behaviour

Panic with `Command::get_arg_conflicts_with: The passed arg conflicts with an arg unknown to the cmd."

### Expected Behaviour

Return  a vec containing the commands in the "gr" group.

### Additional Context

I encountered this while trying to generate zsh completions for a command that has an arg that conflicts with a group, because clap_complete calls `get_arg_conflicts_with` for each argument when generating zsh completions. 

The correct logic for this must exist somewhere, because as can be seen in the code above, it correctly identifies the conflict when parsing a command containing a conflict with the group.

### Debug Output

_No response_

---

_Label `C-bug` added by @tmccombs on 2022-07-07 07:45_

---

_Comment by @tmccombs on 2022-07-07 08:05_

Also worth pointing out that the documentation states that `get_arg_conflicts_with` gets a "list of all arguments the given argument conflicts with". but from what I can tell, this isn't actually true. It just gets a list of the arguments that are listed as `conflicts_with` for this specific `Arg` value, not other arguments that are specified to conflict with this value.

---

_Comment by @tmccombs on 2022-07-07 08:22_

If I move the `conflicts_with` from the `Arg` to the `ArgGroup`, then it no longer panics, but the zsh shell completion doesn't detect the conflict between the group and the other option. But maybe that should be a separate bug with clap_complete?

---

_Referenced in [clap-rs/clap#3902](../../clap-rs/clap/pulls/3902.md) on 2022-07-09 06:38_

---

_Comment by @epage on 2022-07-11 20:38_

> If I move the conflicts_with from the Arg to the ArgGroup, then it no longer panics, but the zsh shell completion doesn't detect the conflict between the group and the other option. But maybe that should be a separate bug with clap_complete?

Yes, that is a separate bug.

---

_Closed by @epage on 2022-07-14 14:37_

---

_Comment by @epage on 2022-07-14 14:39_

v3.2.12 is now released with this

Thanks!

---
