```yaml
number: 38
title: .gitignore whitespace bug
type: issue
state: closed
author: msarchet
labels:
  - bug
assignees: []
created_at: 2016-09-23T23:12:39Z
updated_at: 2016-09-25T00:56:40Z
url: https://github.com/BurntSushi/ripgrep/issues/38
synced_at: 2026-01-12T18:23:11Z
```

# .gitignore whitespace bug

---

_@msarchet_

If an ignored path in a .gitignore has whitespace afterwards

```
node_modules/   <--- whitespace
```

ripgrep with still search the `node_modules/` folder, removing the whitespace fixes this problem.


---

_Comment by @BurntSushi on 2016-09-24 02:05_

I buy it. From `man gitignore`:

```
Trailing spaces are ignored unless they are quoted with backslash ("\").
```


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:05_

---

_Closed by @BurntSushi on 2016-09-25 00:56_

---
