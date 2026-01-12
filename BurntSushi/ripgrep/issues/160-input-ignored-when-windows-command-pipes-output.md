```yaml
number: 160
title: Input ignored when Windows command pipes output to ripgrep
type: issue
state: closed
author: Praful
labels: []
assignees: []
created_at: 2016-10-10T02:13:30Z
updated_at: 2016-10-10T16:25:05Z
url: https://github.com/BurntSushi/ripgrep/issues/160
synced_at: 2026-01-12T18:23:11Z
```

# Input ignored when Windows command pipes output to ripgrep

---

_@Praful_

When you pipe output of a Windows command to `rg`, `rg`ignores it and searches folders.

For example, the output of `dir` is ignored by `rg` in:

```
dir /s |rg -i test
```

If `rg` is replaced by `findstr`, `ack` or `pt`, the correct output is displayed.

I have tried this only on Windows 10.


---

_Comment by @BurntSushi on 2016-10-10 16:25_

This is a duplicate of #94.

For a work-around, use `dir /s | rg -i test -`.


---

_Closed by @BurntSushi on 2016-10-10 16:25_

---
