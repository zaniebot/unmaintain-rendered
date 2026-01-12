```yaml
number: 580
title: "\"-w\" does not work if pattern contains \"|\""
type: issue
state: closed
author: jpetkau
labels: []
assignees: []
created_at: 2017-08-22T22:36:13Z
updated_at: 2017-08-22T22:44:17Z
url: https://github.com/BurntSushi/ripgrep/issues/580
synced_at: 2026-01-12T16:13:22Z
```

# "-w" does not work if pattern contains "|"

---

_@jpetkau_

For example, the command `rg -w 'aa|ee|ii|oo|uu'` incorrectly matches `foo` and `aardvark`.

It appears that `-w` is implemented by surrounding the pattern with `\b`, so the above is equivalent to `\baa|ee|ii|oo|uu\b`, when it should be equivalent to `\baa\b|\bee\b|\bii\b|\boo\b|\buu\b` or `\b(aa|ee|ii|oo|uu)\b`.


---

_Comment by @BurntSushi on 2017-08-22 22:44_

This is fixed in master (see #516).

---

_Closed by @BurntSushi on 2017-08-22 22:44_

---
