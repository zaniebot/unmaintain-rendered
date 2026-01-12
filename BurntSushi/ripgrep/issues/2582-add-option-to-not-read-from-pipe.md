```yaml
number: 2582
title: Add option to not read from pipe
type: issue
state: closed
author: AndersMunch
labels: []
assignees: []
created_at: 2023-08-15T08:40:26Z
updated_at: 2023-08-16T12:05:26Z
url: https://github.com/BurntSushi/ripgrep/issues/2582
synced_at: 2026-01-12T16:13:24Z
```

# Add option to not read from pipe

---

_@AndersMunch_

#### Describe your feature request

I would like an option to tell ripgrep, that despite being connected to a pipe, it should not search standard input, so that it will do a normal recursive search.

Don't know about the name.  ``--no-pipe``, perhaps?

#### Motivation

ripgrep does not work from within Emacs when used as the grep-command on Windows.  It exits immediately with error code 1, or hangs.
I suspect this is related to issue #951, something to do with stdin being a "fifo"?  I'm not sure what that's supposed to mean, from what I can see Emacs is just running a bog standard inferior process - and indeed trying to run ripgrep as a Python subprocess exhibits the same problem, this hangs:

```python
import subprocess
subprocess.check_output(['rg','SomeSearchTerm'])
```

But never mind the reason, I would just like an option to fix it.

---

_Comment by @BurntSushi on 2023-08-15 12:34_

Why doesn't `rg foo ./` work?

---

_Comment by @AndersMunch on 2023-08-16 11:43_

Good point.  `rg foo .` does work.  I just never used the second argument, so I didn't think of it.

---

_Comment by @BurntSushi on 2023-08-16 12:05_

The key insight is that by omitting the path argument you are basically saying, "ripgrep, please guess whether to search the current working directory or stdin." Due to idiosyncracies in how processes are created in various contexts, it is impossible for ripgrep to guess correctly 100% of the time.

---

_Locked by @ghost on 2023-08-16 12:05_

---
