```yaml
number: 583
title: zsh completion failed
type: issue
state: closed
author: misery
labels: []
assignees: []
created_at: 2017-08-24T13:42:33Z
updated_at: 2017-08-24T14:04:13Z
url: https://github.com/BurntSushi/ripgrep/issues/583
synced_at: 2026-01-12T16:13:22Z
```

# zsh completion failed

---

_@misery_

Since 0.6.0 zsh completion is broken.

Just type "rg" and double press TAB key:
```
$ rg 
_rg: parse error near `)'
```
zsh version: 5.4.1

If I use _rg zsh file from 0.5.2 in 0.6.0 it still works.


---

_Comment by @okdana on 2017-08-24 14:03_

Probably the same issue as #582: It seems that the Arch packager made a mistake in their install script, so that the PowerShell completion function is installed instead of the zsh one.

Here's the change that broke it:

https://git.archlinux.org/svntogit/community.git/commit/trunk?h=packages/ripgrep&id=370020d2f6db7b71c053667c5f2ef073f77f6a61

And here's a bug ticket for it:

https://bugs.archlinux.org/task/55266

As a work-around you can install the completion file directly from the repo:

```
curl -sSL 'https://raw.githubusercontent.com/BurntSushi/ripgrep/master/complete/_rg' | sudo tee /usr/share/zsh/site-functions/_rg
```

---

_Comment by @misery on 2017-08-24 14:04_

Ah, thanks... sorry for the duplicate!

---

_Closed by @misery on 2017-08-24 14:04_

---
