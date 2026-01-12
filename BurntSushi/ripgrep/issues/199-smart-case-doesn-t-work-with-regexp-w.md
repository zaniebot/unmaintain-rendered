```yaml
number: 199
title: "--smart-case doesn't work with --regexp/-w"
type: issue
state: closed
author: alejandro5042
labels:
  - bug
assignees: []
created_at: 2016-10-28T17:00:18Z
updated_at: 2016-11-06T17:07:56Z
url: https://github.com/BurntSushi/ripgrep/issues/199
synced_at: 2026-01-12T18:23:11Z
```

# --smart-case doesn't work with --regexp/-w

---

_@alejandro5042_

Smart casing works great if you use --fixed-strings. However, once you use --regexp or -w, it falls apart:

For example, this code does not match "ToCsvFile" in any file.

```
rg --smart-case --regexp "\btocsvfile"
```

Same with -w. Doesn't work:

```
rg --smart-case -w "tocsvfile"
```

But begins matching with --ignore-case.


---

_Comment by @alejandro5042 on 2016-10-28 17:00_

Version 0.2.3.


---

_Label `bug` added by @BurntSushi on 2016-10-28 17:27_

---

_Comment by @BurntSushi on 2016-10-28 17:30_

This is indeed a bug. It's probably related to literal optimizations that are ignoring the smart case flag. For example, this works even though it's using a regex:

```
[andrew@Cheetah tmp] cat foo
ToCSVFile
tocsvfile
[andrew@Cheetah tmp] rg '^\p{Ll}+$' foo
2:tocsvfile
[andrew@Cheetah tmp] rg -S '^\p{Ll}+$' foo
1:ToCSVFile
2:tocsvfile
```

(Although, I wonder if making pure regexes case insensitive even when smart case is enabled is desirable. Maybe it should only activate if there's at least one literal byte? Not sure, but that's a separate issue I guess.)


---

_Closed by @BurntSushi on 2016-11-06 17:07_

---
