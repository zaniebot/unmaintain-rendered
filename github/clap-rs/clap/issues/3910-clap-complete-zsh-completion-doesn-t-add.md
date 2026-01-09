---
number: 3910
title: "clap_complete: zsh completion doesn't add conflicts for group"
type: issue
state: open
author: tmccombs
labels:
  - C-bug
  - A-completion
  - E-easy
assignees: []
created_at: 2022-07-12T07:59:15Z
updated_at: 2024-05-06T06:07:35Z
url: https://github.com/clap-rs/clap/issues/3910
synced_at: 2026-01-07T13:12:20-06:00
---

# clap_complete: zsh completion doesn't add conflicts for group

---

_Issue opened by @tmccombs on 2022-07-12 07:59_

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
use clap_complete::{generate, shell};

fn main() {
    let mut app = Command::new("test")
        .group(ArgGroup::new("gr").args(&["a", "b"]).conflicts_with("c"))
        .arg(Arg::new("a").short('a'))
        .arg(Arg::new("b").short('b'))
        .arg(Arg::new("c").short('c'))
        .arg(&c);
    app.build();
        
   generate(shell::Zsh, &mut app, app.get_name(), &mut std::io::stdout());
}
```


### Steps to reproduce the bug with the above code

Run code

### Actual Behaviour

The zsh completions generated don't account for the conflict between the group and option c. 

### Expected Behaviour

The zsh completions should specify that the options in the group conflict with option c.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @tmccombs on 2022-07-12 07:59_

---

_Label `A-completion` added by @epage on 2022-07-13 14:09_

---

_Label `E-easy` added by @epage on 2022-07-13 14:09_

---

_Comment by @kazuminn on 2024-05-04 14:46_

Is this problem still unsolved?

---

_Referenced in [Yamato-Security/hayabusa#1340](../../Yamato-Security/hayabusa/pulls/1340.md) on 2024-05-04 14:50_

---

_Comment by @epage on 2024-05-04 16:26_

With this being open, likely yes.

---

_Comment by @kazuminn on 2024-05-05 13:50_

@epage Thanks for your replay.
Could you fix this issue or tell me that how to fix this issue.
Then I can write and contribute some code of the clap_complete.
Do you have an idea of fixing?

---

_Comment by @epage on 2024-05-06 03:16_

I am  not a zsh user and do not know its completion system to be able to say how to do this.  I assume tmccombs verified that it supports conflicts but I'm not even sure.  If it doesn't, then this is more likely to just be rejected.

We are looking at changing our completion system which would make it easier to implement these rich features and do it once for all shells.  See #3166.

---

_Comment by @tmccombs on 2024-05-06 03:46_

zsh does support conflicts.

You can see an example of how it is done in the `fd` zsh completions (which are currently manually maintained), such as [here](https://github.com/sharkdp/fd/blob/master/contrib/completion/_fd#L91) where we declare that `--list-details`  conflicts with the `abs-path`, `null-sep`, `max-results`, and  `exec-cmds` groups. 

Basically, when calling `_arguments` you put a list of conflicting options or option groups in parenthesis before the name of the option. 

From [zshcompsys(1)][1]:

> Each of the forms above may be preceded by a list in parentheses of option names and argument numbers.  If the given option is on the command line, the options  and  arguments  indicated  in  parentheses  will not be offered.  For example, ‘(-two -three 1)-one:...' completes the option ‘-one'; if this appears on the command line, the options -two and   -three and the first ordinary argument will not be completed after it.
> ...
> Options can be grouped to simplify exclusion lists. A group is introduced with ‘+' followed by a name for the group in the subsequent word. Whole groups can then be referenced in an exclusion list or a group name can be used to disambiguate between two forms of the same option.

[1]: https://man.archlinux.org/man/zshcompsys.1

---

_Comment by @kazuminn on 2024-05-06 06:07_

I have a panic with OSS hayabusa.

https://github.com/Yamato-Security/hayabusa/pull/1340

```
thread 'main' panicked at .cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.2/src/builder/command.rs:3702:21:
Command::get_arg_conflicts_with: The passed arg conflicts with an arg unknown to the cmd
```

---
