```yaml
number: 2724
title: Add Vuejs as a file list to type-list
type: issue
state: closed
author: DMunkei
labels: []
assignees: []
created_at: 2024-01-31T14:19:49Z
updated_at: 2024-01-31T14:21:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2724
synced_at: 2026-01-12T16:13:24Z
```

# Add Vuejs as a file list to type-list

---

_@DMunkei_

When I'm trying to limit my search only on the filetype `.vue` using the `-t` flag it doesn't work. 

I get the following error:
```zsh
➜ rg -tvue LecturerBelongsToCourse
rg: unrecognized file type: vue
```
I checked inside of the found types with `rg --type-list` the output shows that it's not added. Here's the section detailing filetypes starting with the letter **v**
```bash
v: *.v, *.vsh
vala: *.vala
vb: *.vb
vcl: *.vcl
verilog: *.sv, *.svh, *.v, *.vh
vhdl: *.vhd, *.vhdl
vim: *.vim, .gvimrc, .vimrc, _gvimrc, _vimrc, gvimrc, vimrc
vimscript: *.vim, .gvimrc, .vimrc, _gvimrc, _vimrc, gvimrc, vimrc
```

I believe it would be a nice addition to add `Vuejs` files to be handled by ripgrep :).

I'd be willing to have a go at adding it, if it's not something that's too complicated? Or with some guidance. 

---

_Comment by @BurntSushi on 2024-01-31 14:21_

Thanks for the report.

I don't track issues for file type requests. Adding them to ripgrep [is trivial](https://github.com/BurntSushi/ripgrep/pull/2678), so I'd rather folks just submit PRs adding them.

---

_Closed by @BurntSushi on 2024-01-31 14:21_

---
