```yaml
number: 47
title: Using --help with aliased rg
type: issue
state: closed
author: kaushalmodi
labels:
  - bug
assignees: []
created_at: 2016-09-24T04:06:48Z
updated_at: 2016-09-26T01:37:24Z
url: https://github.com/BurntSushi/ripgrep/issues/47
synced_at: 2026-01-12T18:23:11Z
```

# Using --help with aliased rg

---

_@kaushalmodi_

I have aliased `rg` to `rg --follow`.

After that `rg --help` gives this error:

```
Invalid arguments.

Usage: rg [options] -e PATTERN ... [<path> ...]
       rg [options] <pattern> [<path> ...]
       rg [options] --files [<path> ...]
       rg [options] --type-list
       rg --help
       rg --version
```

So I need to remember to use `\rg --help` when I need to see help.

The `--help` arg probably needs to ignore all other args provided to `rg`.


---

_Comment by @BurntSushi on 2016-09-24 04:10_

Yeah, I see, Docopt doesn't make this especially convenient. It looks like I'll need to hand-hold Docopt through this one. (Instead of letting it figure out when to show the usage message itself.)


---

_Label `bug` added by @BurntSushi on 2016-09-24 04:10_

---

_Closed by @BurntSushi on 2016-09-25 01:13_

---

_Comment by @kaushalmodi on 2016-09-26 01:37_

Thanks for the fix. I confirm the fix.


---
