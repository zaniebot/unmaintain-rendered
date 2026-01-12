```yaml
number: 2062
title: git setting core.excludesFile is ignored if placed inside an include.path of ~/.gitconfig
type: issue
state: closed
author: cdleonard
labels:
  - duplicate
assignees: []
created_at: 2021-11-13T07:54:10Z
updated_at: 2021-11-15T14:49:08Z
url: https://github.com/BurntSushi/ripgrep/issues/2062
synced_at: 2026-01-12T16:13:24Z
```

# git setting core.excludesFile is ignored if placed inside an include.path of ~/.gitconfig

---

_@cdleonard_

#### What version of ripgrep are you using?

ripgrep 11.0.2

#### How did you install ripgrep?

apt-get install ripgrep

#### What operating system are you using ripgrep on?

Linux, Mac

#### Describe your bug.

Give a high level description of the bug.

#### What are the steps to reproduce the behavior?

I have multiple per-machine gitconfig files with sections like this:

```
[include]                                                                                                                                                                                                               
           path = ~/.local/lib/private-scripts/common.gitconfig 
```

and inside `common.gitconfig` I have this section:
```
[core]                                                                                                                                                                                                                  
           excludesFile = ~/.gitignore
```

The excludesFile setting inside `common.gitconfig` is ignored, I have to place this snippet in each per-machine gitconfig file instead.

#### What is the actual behavior?

The excludesFile setting inside `common.gitconfig` is ignored.

#### What is the expected behavior?

Looking through the code it seems that ripgrep has a custom regex-based parser for gitconfig files which does not support includes.

I initially reported this for vscode https://github.com/microsoft/vscode/issues/134322 but after a little digging this behavior can be reproduced with standalone `rg` and after grepping for "excludesFile" the causes seems obvious: https://github.com/BurntSushi/ripgrep/blob/master/crates/ignore/src/gitignore.rs#L591

An reasonably easy fix would be to call `git config core.excludesFile` instead.

---

_Comment by @cdleonard on 2021-11-13 08:30_

While lacking include support is a legitimate bug there is no good reason for me to use a custom core.excludesFile settings instead of the default ~/.config/git/ignore. I have just been doing so since before ~/.config/git/ignore was supported by git.

---

_Comment by @BurntSushi on 2021-11-15 14:49_

Sadly, dupe of #1014.

---

_Closed by @BurntSushi on 2021-11-15 14:49_

---

_Label `duplicate` added by @BurntSushi on 2021-11-15 14:49_

---
