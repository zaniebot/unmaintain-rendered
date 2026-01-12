```yaml
number: 535
title: Filenames invisible in PowerShell
type: issue
state: closed
author: mcandre
labels:
  - duplicate
assignees: []
created_at: 2017-06-30T17:50:39Z
updated_at: 2017-06-30T19:15:06Z
url: https://github.com/BurntSushi/ripgrep/issues/535
synced_at: 2026-01-12T16:13:22Z
```

# Filenames invisible in PowerShell

---

_@mcandre_

The default ripgrep color scheme renders well in Command Prompt and Git Bash, but filenames are perfectly invisible against the PowerShell default background color. Could we improve the ripgrep color scheme to play well with PowerShell?

As a workaround, I'm explicitly calling rg with `--color never` for now :/


---

_Comment by @elirnm on 2017-06-30 18:18_

See https://github.com/BurntSushi/ripgrep/issues/342. A workaround is `--colors "path:style:intense"` (or `--colors "path:whatever:whatever"`). There's a section in the readme on setting up an alias in Powershell if you want to always use that.

---

_Comment by @BurntSushi on 2017-06-30 19:15_

This has been reported a couple of times and appears to be a dupe of #342.

> Could we improve the ripgrep color scheme to play well with PowerShell?

Anyone is welcome to propose a solution.

---

_Closed by @BurntSushi on 2017-06-30 19:15_

---

_Label `duplicate` added by @BurntSushi on 2017-06-30 19:15_

---
