```yaml
number: 2523
title: ty resolves symlinks when going to source
type: issue
state: open
author: Boon-in-Oz
labels:
  - server
assignees: []
created_at: 2026-01-15T21:11:10Z
updated_at: 2026-01-16T08:01:40Z
url: https://github.com/astral-sh/ty/issues/2523
synced_at: 2026-01-16T08:55:00Z
```

# ty resolves symlinks when going to source

---

_@Boon-in-Oz_

### Summary

The standard setup at my workplace is to junction our development directory. So I work in C:\whatever but the physical location of all my files is actually S:\whatever. In VS Code, when I hit F12 on a symbol, ty opens the source code  in the S:\whatever location.

I briefly mentioned it [in issue 1967](https://github.com/astral-sh/ty/issues/1967#issuecomment-3663117349), but it turned out not to be the source of that bug.

The `python-environment-tools` project just fixed this bug in their locator: https://github.com/microsoft/python-environment-tools/pull/277

Any chance you could implement a similar fix in `ty` please?

### Version

0.0.12

---

_Label `server` added by @AlexWaygood on 2026-01-15 21:16_

---

_Comment by @MichaReiser on 2026-01-16 08:01_

> So I work in C:\whatever but the physical location of all my files is actually S:\whatever. In VS Code, when I hit F12 on a symbol, ty opens the source code in the S:\whatever location.

I think you mixed up the path here as `C:\whatever` is both the pyhysical and actual location. Can you try to describe the problem again. What behavior are you seen and what behavior would do expect

---
