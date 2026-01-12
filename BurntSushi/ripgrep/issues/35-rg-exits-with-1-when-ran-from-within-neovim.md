```yaml
number: 35
title: rg exits with 1 when ran from within neovim
type: issue
state: closed
author: flybayer
labels:
  - bug
assignees: []
created_at: 2016-09-23T20:56:53Z
updated_at: 2016-09-25T18:45:28Z
url: https://github.com/BurntSushi/ripgrep/issues/35
synced_at: 2026-01-12T18:23:11Z
```

# rg exits with 1 when ran from within neovim

---

_@flybayer_

I'm trying to use `rg` with [ag.vim](https://github.com/vim-scripts/ag.vim). I have the following in my vim config:

```
let g:ag_prg="rg --no-heading --vimgrep"
```

However, it never returns results. 

It turns out that when trying to run it from the vim command line it exits with `1` instead of giving results:
![image](https://cloud.githubusercontent.com/assets/8813276/18800998/e5ab0398-81a5-11e6-9eca-6f12749ebef6.png)

The same thing works fine with `ag`:
![image](https://cloud.githubusercontent.com/assets/8813276/18801015/fa88674c-81a5-11e6-8827-e2b818e44dbd.png)

Running `rg --no-heading --vimgrep` from the terminal works fine.

Here's trying to run `rg` in vim with `--debug`:
![image](https://cloud.githubusercontent.com/assets/8813276/18801058/3ccca6e0-81a6-11e6-8ca4-7244dabd05dd.png)

Any ideas?


---

_Comment by @BurntSushi on 2016-09-23 21:08_

Are you sure your search is taking place in the right directory and actually returns results? I can run `:!rg --vimgrep foo` in my vim just fine.

It might help to try running `:!rg --files` to see precisely what it's trying to search.


---

_Comment by @flybayer on 2016-09-23 21:22_

@BurntSushi This is weird.. 

`:!rg --files` prints out the files properly

However, `:!rg appName` returns `1`


---

_Comment by @elasticdog on 2016-09-23 21:23_

Just to confirm the reported issue, I'm also seeing this behavior with the `shell returned 1` from `rg` where `ag` works as expected. This is using NeoVim (NVIM v0.1.6-120-g4a6b4bb) on OS X 10.11.6 for what it's worth. When I add the `--files` flag, it does output the expected list of files, but always exits with 1, as far as my limited testing has shown.


---

_Comment by @flybayer on 2016-09-23 21:24_

Thanks @elasticdog 

I'm using NeoVim on Ubuntu 14.04 with oh-my-zsh.


---

_Comment by @BurntSushi on 2016-09-23 21:40_

Interesting, I can finally reproduce this in neovim (Linux), but vim works fine (Linux, Mac).

Only thing I can think of is that `rg` thinks its getting `stdin` piped, so it tried to read it, finds nothing, and reports failure. I'll dig into it.


---

_Comment by @little-dude on 2016-09-23 22:07_

could it be due to [this](https://github.com/neovim/neovim/issues/1496#issuecomment-63691483)? Quoting:

> This is not a bug, it is the new behavior of bang commands: We no longer spawn the program with it's stdout connected to Nvim tty, instead we open a pipe, read output and display to the user. This is the only way the bang commands will be consistent across UIs, so programs designed to be used interactively from the terminal will no longer work from inside nvim.


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:06_

---

_Comment by @jonboiser on 2016-09-24 15:12_

I'm running into something similar trying to add `rg` to [atom-fuzzy-grep](https://github.com/geksilla/atom-fuzzy-grep).

`rg --ignore-case  --with-filename --no-heading --column` produces output compatible to `ag`.

Testing with `rg --files` works fine, but with any other flags, the process exits with code 1. I'm not a Node expert, but I think the library author sets the `stdio` options for ChildProcess to ignore `stdin` [link](https://github.com/geksilla/atom-fuzzy-grep/blob/master/lib/runner.coffee#L25).


---

_Renamed from "rg exits with 1 when ran from within vim" to "rg exits with 1 when ran from within neovim" by @BurntSushi on 2016-09-24 15:43_

---

_Closed by @BurntSushi on 2016-09-25 15:15_

---

_Comment by @BurntSushi on 2016-09-25 15:16_

This should be fixed now. `rg` was being a bit too aggressive with trying to read from `stdin`. It will now only try if it's given a file or a FIFO.


---

_Comment by @remi on 2016-09-25 18:41_

@BurntSushi Awesome! Do you plan to release a new version soon? Thank you! 😄 


---

_Comment by @BurntSushi on 2016-09-25 18:45_

I'd like to get a release out today. I'm just trying to cram as many fixes in as I can. :-)


---
