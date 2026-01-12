```yaml
number: 3045
title: Flag to require matching all patterns, not just one
type: issue
state: closed
author: lolbinarycat
labels: []
assignees: []
created_at: 2025-05-11T20:35:55Z
updated_at: 2025-05-11T22:09:25Z
url: https://github.com/BurntSushi/ripgrep/issues/3045
synced_at: 2026-01-12T16:13:25Z
```

# Flag to require matching all patterns, not just one

---

_@lolbinarycat_

Oftentimes I want to look for a line that contains multiple strings, but I don't know what order they appear in.

Currently this can be achieved by piping multiple `rg` invocations together, but this doesn't work with flags like `-B` or `-A`.

I propose adding a flag (say `--match-all`) that requires that a line must match all patterns from pattern files, `-e`, etc, instead of just one.

I can work on a PR implementing this, if desired.

---

_Closed by @BurntSushi on 2025-05-11 22:09_

---
