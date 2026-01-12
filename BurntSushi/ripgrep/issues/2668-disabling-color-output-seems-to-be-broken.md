```yaml
number: 2668
title: Disabling color output seems to be broken
type: issue
state: closed
author: wallentx
labels:
  - invalid
assignees: []
created_at: 2023-11-30T14:24:43Z
updated_at: 2023-11-30T14:33:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2668
synced_at: 2026-01-12T16:13:24Z
```

# Disabling color output seems to be broken

---

_@wallentx_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

My screenshot shows 14.0.2 (what is available in the Arch repo), but I also uninstalled from pacman, and did a `hash -r && cargo install ripgrep` to replicate the same behavior with 14.0.3


### How did you install ripgrep?

Pacman & cargo

### What operating system are you using ripgrep on?

Arch 6.6.2-arch1-1

### Describe your bug.

![Screenshot_20231130-075318](https://github.com/BurntSushi/ripgrep/assets/8990544/8277b5d3-0c69-46ca-9634-bd6284f404c1)

It also does not seem to obey the presence of a `NO_COLOR` environment variable.

### What are the steps to reproduce the behavior?

See the screenshot above

### What is the actual behavior?

☝️

### What is the expected behavior?

Colors option flags should work, and passing `--colors path:none --colors line:none --colors column:none --colors match:none` should not have `^[[0m` "leftovers"

---

_Comment by @BurntSushi on 2023-11-30 14:33_

You're passing `-p/--pretty`, which is an alias for `--color=always --heading --line-number`. You're also passing it last, so it overrides everything else.

This may have accidentally worked as desired in ripgrep 13, but ripgrep 14 is now much more consistent about flag overrides based on their relative position to one another.

---

_Closed by @BurntSushi on 2023-11-30 14:33_

---

_Label `invalid` added by @BurntSushi on 2023-11-30 14:33_

---
