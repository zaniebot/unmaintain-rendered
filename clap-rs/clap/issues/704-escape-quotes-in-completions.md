---
number: 704
title: Escape quotes in completions
type: issue
state: closed
author: tilpner
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2016-10-25T00:56:55Z
updated_at: 2018-08-02T03:29:55Z
url: https://github.com/clap-rs/clap/issues/704
synced_at: 2026-01-10T01:26:34Z
---

# Escape quotes in completions

---

_Issue opened by @tilpner on 2016-10-25 00:56_

[rustup just exposed completion generation](https://internals.rust-lang.org/t/beta-testing-rustup-rs/3316/196), but fish complains about the generated completions

```
Unexpected end of string, quotes are not balanced
~/.config/fish/completions/rustup.fish (line 188): complete -c rustup -n '__fish_using_command rustup help' -s V -l version -d 'Prints version information'

                  ^
from sourcing file ~/.config/fish/completions/rustup.fish
    called on standard input

in command substitution
    called on standard input

source: Error while reading file “/home/till/.config/fish/completions/rustup.fish”
```

The completions don't escape the help items, leading to the [' in "Don't"](https://gist.github.com/tilpner/40686d60b2e045829f3f4d4dcaf7f802#file-rustup-fish-L38) ending the help string, breaking the rest of the file.

ping @brson


---

_Referenced in [rust-lang/rustup#781](../../rust-lang/rustup/issues/781.md) on 2016-10-25 00:58_

---

_Comment by @kbknapp on 2016-10-25 03:57_

Thanks for the heads up! I should have this fixed first thing tomorrow :+1:


---

_Label `T: bug` added by @kbknapp on 2016-10-25 03:58_

---

_Label `P1: urgent` added by @kbknapp on 2016-10-25 03:58_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-25 03:58_

---

_Label `C: completion gen` added by @kbknapp on 2016-10-25 03:58_

---

_Added to milestone `2.16.2` by @kbknapp on 2016-10-25 03:59_

---

_Comment by @kbknapp on 2016-10-25 14:39_

This is fixed in #705 

Once it merges I'll put v2.16.2 on crates.io


---

_Closed by @homu on 2016-10-25 15:33_

---
