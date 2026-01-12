```yaml
number: 2143
title: " ripgrep 13.0.0 never exits when spawned in neovim"
type: issue
state: closed
author: Azeirah
labels:
  - duplicate
assignees: []
created_at: 2022-02-11T23:47:18Z
updated_at: 2022-02-12T00:01:43Z
url: https://github.com/BurntSushi/ripgrep/issues/2143
synced_at: 2026-01-12T16:13:24Z
```

#  ripgrep 13.0.0 never exits when spawned in neovim

---

_@Azeirah_

#### What version of ripgrep are you using?

13.0.0

#### How did you install ripgrep?

Followed the recent installation guidelines from the readme.

```
$ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep_13.0.0_amd64.deb
$ sudo dpkg -i ripgrep_13.0.0_amd64.deb
```

#### What operating system are you using ripgrep on?

Ubuntu

```
$ uname -a
Linux ________ 5.13.0-28-generic #31~20.04.1-Ubuntu SMP Wed Jan 19 14:08:10 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

#### Describe your bug.

When using neovim to spawn a ripgrep process, it hangs indefinitely.
This is the case for version 13.0.0, but NOT for version 12.0.1, the latter of which functions as expected.

#### What are the steps to reproduce the behavior?

I assume familiarity with neovim, although I wrote these reproduction steps with additional detail for those who're less familiar with neovim.

1. Install the `plenary` plugin in neovim. https://github.com/nvim-lua/plenary.nvim (easiest is to use a package manager such as [packer](https://github.com/wbthomason/packer.nvim))
2. Execute the following command. That is, press `:` and type in everything after the `:` below. Press enter to execute.

`:lua require('plenary.job'):new({command='rg',args={'some'}}).sync()`

Using version 13.0.0, this will fail with a timeout error, whereas with version 12.0.1 it will succeed as expected.

#### What is the actual behavior?

Ripgrep does not exit at all.

#### What is the expected behavior?

Ripgrep should exit, possibly similar to version 12.0.1

#### What else have you tried?

I have tried the reproduction steps with both versions 6.1 and 6.0 of neovim, this does fix the issue.

I have also tried the reproduction steps in both the default shell in ubuntu (gnome-shell I believe?) and the kitty shell. Again, this does not influence the bug.

---

_Renamed from " ripgrep 13 never exits when spawned in neovim" to " ripgrep 13.0.0 never exits when spawned in neovim" by @Azeirah on 2022-02-11 23:47_

---

_Comment by @BurntSushi on 2022-02-12 00:01_

Duplicate of #1892.

See this comment in particular: https://github.com/BurntSushi/ripgrep/issues/1892#issuecomment-860270717

---

_Closed by @BurntSushi on 2022-02-12 00:01_

---

_Label `duplicate` added by @BurntSushi on 2022-02-12 00:01_

---
