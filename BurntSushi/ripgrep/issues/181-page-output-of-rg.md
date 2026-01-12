```yaml
number: 181
title: Page output of rg,
type: issue
state: closed
author: sumonto
labels: []
assignees: []
created_at: 2016-10-14T21:16:18Z
updated_at: 2016-10-14T21:32:09Z
url: https://github.com/BurntSushi/ripgrep/issues/181
synced_at: 2026-01-12T18:23:11Z
```

# Page output of rg,

---

_@sumonto_

Hi,
Wondering if it is possible to add an option to page the output of rg instead of me having to do 
rg <string> --color always | less -r

Thanks.


---

_Comment by @BurntSushi on 2016-10-14 21:32_

Dupe of #86.

Specifically, try putting this in your `$HOME/bin`:

```
#!/bin/sh

/path/to/rg -p "$@" | less -RFX
```


---

_Closed by @BurntSushi on 2016-10-14 21:32_

---
