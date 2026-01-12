```yaml
number: 1526
title: "[Question] Search for a file that contains a string"
type: issue
state: closed
author: msclp
labels:
  - invalid
assignees: []
created_at: 2020-03-19T18:37:00Z
updated_at: 2020-03-19T18:44:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1526
synced_at: 2026-01-12T16:13:23Z
```

# [Question] Search for a file that contains a string

---

_@msclp_

#### What version of ripgrep are you using?

12.0.0

#### How did you install ripgrep?

with brew

#### What operating system are you using ripgrep on?

macOS 10.15.3

#### Describe your question, feature request, or bug.

How to use `vim`picker` with `ripgrep` to  search a file that contains a specified string

This does not work well
```
nmap <leader>a :call picker#File("rg --column --line-number --no-heading --color=never --smart-case ''", 'edit')<cr>
```

It would be awesome if it would work same as `<Plug>(PickerEdit)`

---

_Label `invalid` added by @BurntSushi on 2020-03-19 18:44_

---

_Comment by @BurntSushi on 2020-03-19 18:44_

This doesn't look like a ripgrep question to me, sorry. If you have a ripgrep question, then please ask it without respect to other tools. Otherwise, it sounds like you need a support forum for vim or whatever plugin you're using.

---

_Closed by @BurntSushi on 2020-03-19 18:44_

---

_Comment by @msclp on 2020-03-19 18:44_

Sorry, I just noticed I asked here, not in `vim-picker` 

---
