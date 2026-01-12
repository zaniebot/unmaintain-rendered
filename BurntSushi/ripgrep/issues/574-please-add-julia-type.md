```yaml
number: 574
title: Please add julia type
type: issue
state: closed
author: pintergreg
labels: []
assignees: []
created_at: 2017-08-16T10:06:08Z
updated_at: 2017-08-16T14:46:16Z
url: https://github.com/BurntSushi/ripgrep/issues/574
synced_at: 2026-01-12T16:13:22Z
```

# Please add julia type

---

_@pintergreg_

[Julia](https://julialang.org/) uses the `.jl` extension and it would be great if I could use a Julia type filter. I figured out `*.jl` is listed in lisp types, so I'm using `-tlisp` currently, but a `-tjl` would be more handy such as `-tpy` for python.
Thanks

---

_Comment by @BurntSushi on 2017-08-16 10:35_

It already exists: https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/types.rs#L147

If you want `jl` specifically, then please submit a PR.

---

_Comment by @pintergreg on 2017-08-16 12:12_

You're right. I did not check the master, only the latest release I'm using (0.5.2) nor specified version.

---

_Closed by @BurntSushi on 2017-08-16 14:46_

---
