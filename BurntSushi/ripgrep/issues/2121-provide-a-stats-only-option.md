```yaml
number: 2121
title: "Provide a `--stats-only` option"
type: issue
state: closed
author: fpdotmonkey
labels: []
assignees: []
created_at: 2022-01-10T21:16:24Z
updated_at: 2022-01-10T21:38:07Z
url: https://github.com/BurntSushi/ripgrep/issues/2121
synced_at: 2026-01-12T16:13:24Z
```

# Provide a `--stats-only` option

---

_@fpdotmonkey_

This is a feature that exists in SilverSearcher.  From its man page,

```
--stats-only
        Print stats (files scanned, time taken, etc) and nothing else.
```

This would do exactly what `--stats` does now, but it would also disable output of matches, headers, files, etc.  Anything that isn't the stats block.


---

_Comment by @BurntSushi on 2022-01-10 21:21_

Could you say why `--stats -q` is insufficient for your use case?

---

_Comment by @fpdotmonkey on 2022-01-10 21:36_

Well largely because I didn't catch that in the man page.  Thank you!

---

_Closed by @fpdotmonkey on 2022-01-10 21:36_

---

_Comment by @fpdotmonkey on 2022-01-10 21:38_

Although actually the man page is a little misleading for `--quiet`.  It reads "Do not print anything to stdout", however `--stats` prints to stdout

---
