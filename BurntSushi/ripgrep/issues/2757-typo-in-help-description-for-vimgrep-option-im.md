```yaml
number: 2757
title: "Typo in help description for --vimgrep option: \"im\" instead of \"in\""
type: issue
state: closed
author: kcollett
labels:
  - bug
assignees: []
created_at: 2024-03-17T20:34:31Z
updated_at: 2024-03-17T20:45:20Z
url: https://github.com/BurntSushi/ripgrep/issues/2757
synced_at: 2026-01-12T16:13:24Z
```

# Typo in help description for --vimgrep option: "im" instead of "in"

---

_@kcollett_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macOS 14.4

### Describe your bug.

In the output of rg -h, there is a typo in the description of the --vimgrep option: "Print results im a vim compatible format." ("im" should be "in".)

### What are the steps to reproduce the behavior?

rg -h

### What is the actual behavior?

See above

### What is the expected behavior?

See above

---

_Comment by @BurntSushi on 2024-03-17 20:44_

Thanks for the issue, but I don't track bugs for typos because they aren't a priority for me to fix unless they obscure meaning. And if I did, I make enough typos that the issue tracker would be overrun. I might get around to fixing this, or just a PR is sufficient.

---

_Closed by @BurntSushi on 2024-03-17 20:44_

---

_Label `bug` added by @BurntSushi on 2024-03-17 20:45_

---
