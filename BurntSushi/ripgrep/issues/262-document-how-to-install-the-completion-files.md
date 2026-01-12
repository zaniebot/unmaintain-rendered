```yaml
number: 262
title: document how to install the completion files
type: issue
state: closed
author: daxim
labels:
  - doc
assignees: []
created_at: 2016-12-01T11:07:35Z
updated_at: 2017-01-07T03:53:37Z
url: https://github.com/BurntSushi/ripgrep/issues/262
synced_at: 2026-01-12T16:13:21Z
```

# document how to install the completion files

---

_@daxim_

The release archive `ripgrep-0.3.1-x86_64-unknown-linux-musl.tar.gz` contains a number of completion files, but no instructions how to use them AFAICS.

---

_Label `doc` added by @BurntSushi on 2016-12-01 12:00_

---

_Comment by @BurntSushi on 2016-12-01 12:00_

I would love to provide documentation, but I don't know how to use them myself. I could figure out bash, but I'm unlikely to figure out the others.

---

_Comment by @kbknapp on 2016-12-02 02:28_

[`rustup` has some simple documentation](https://github.com/rust-lang-nursery/rustup.rs/blob/f6cd385fc58008fd78a4514329d5207c4ff42d57/src/rustup-cli/help.rs#L135-L223).

---

_Comment by @daxim on 2016-12-02 03:29_

Thank you, I used that info in order to write the following documentation patch.

    Shell completions
    =================

    Bash
    ----

    Move `rg.bash-completion` to [`$XDG_CONFIG_HOME/bash_completion`](https://github.com/scop/bash-completion/blob/master/doc/bash_completion.txt#L4) or `/etc/bash_completion.d/`.

    Fish
    ----

    Move `rg.fish` to `$HOME/.config/fish/completions`.

Apply it where you see fit.

---

_Closed by @BurntSushi on 2017-01-07 03:53_

---
