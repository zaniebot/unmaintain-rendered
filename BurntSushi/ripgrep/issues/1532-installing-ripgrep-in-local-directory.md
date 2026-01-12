```yaml
number: 1532
title: Installing ripgrep in local directory
type: issue
state: closed
author: shahid31091
labels:
  - question
assignees: []
created_at: 2020-03-28T01:24:07Z
updated_at: 2020-03-28T02:01:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1532
synced_at: 2026-01-12T16:13:23Z
```

# Installing ripgrep in local directory

---

_@shahid31091_

Hi,

I do not have sudo access but I would like to install ripgrep.

Is there a way to install this in the ~/.local directory?



---

_Comment by @BurntSushi on 2020-03-28 01:32_

```
$ git clone https://github.com/BurntSushi/ripgrep
$ cd ripgrep
$ cargo build --release
$ cp target/release/rg ~/.local/
```

---

_Closed by @BurntSushi on 2020-03-28 01:32_

---

_Label `question` added by @BurntSushi on 2020-03-28 01:32_

---

_Comment by @shahid31091 on 2020-03-28 01:42_

This does not work for me :(
cargo build --release
zsh: command not found: cargo


---

_Comment by @BurntSushi on 2020-03-28 02:01_

Please consider [reading the README](https://github.com/BurntSushi/ripgrep#building) before asking questions.

---
