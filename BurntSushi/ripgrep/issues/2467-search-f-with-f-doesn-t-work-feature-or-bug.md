```yaml
number: 2467
title: "search `-F` with `-F` doesn't work, feature or bug?"
type: issue
state: closed
author: AndyJado
labels: []
assignees: []
created_at: 2023-03-21T10:23:13Z
updated_at: 2023-03-21T11:01:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2467
synced_at: 2026-01-12T16:13:24Z
```

# search `-F` with `-F` doesn't work, feature or bug?

---

_@AndyJado_

version 13.0.0

```sh
rg -h | rg -F '-F'
```


---

_Comment by @s-p-turner on 2023-03-21 10:47_

Would it work correctly if you added the -e/--regexp flag?

rg -h | rg -Fe '-F'

---

_Locked by @ghost on 2023-03-21 11:01_

---
