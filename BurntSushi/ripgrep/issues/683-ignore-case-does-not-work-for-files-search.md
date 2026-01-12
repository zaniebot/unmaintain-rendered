```yaml
number: 683
title: "--ignore-case does not work for files search"
type: issue
state: closed
author: ptzz
labels:
  - wontfix
assignees: []
created_at: 2017-11-16T22:52:07Z
updated_at: 2017-11-16T23:07:59Z
url: https://github.com/BurntSushi/ripgrep/issues/683
synced_at: 2026-01-12T16:13:22Z
```

# --ignore-case does not work for files search

---

_@ptzz_

```
~/tmp$ /bin/ls
AA BB Ba aB
~/tmp$ /usr/local/bin/rg --ignore-case -g '*a*' --files
aB
Ba
~/tmp$ /usr/local/bin/rg --ignore-case -g '*A*' --files
AA
~/tmp$ rg --version
ripgrep 0.7.1
-AVX -SIMD
```
They should give the same output, no?

---

_Comment by @okdana on 2017-11-16 22:54_

`--ignore-case` affects the search pattern, not glob patterns. You can use `--iglob` (instead of `-g`) for that

---

_Label `wontfix` added by @BurntSushi on 2017-11-16 23:07_

---

_Closed by @BurntSushi on 2017-11-16 23:07_

---
