```yaml
number: 1163
title: "^ doesn't match first line when BOM is present with encoding 'auto'"
type: issue
state: closed
author: roblourens
labels:
  - bug
assignees: []
created_at: 2019-01-14T17:47:38Z
updated_at: 2019-01-25T22:19:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1163
synced_at: 2026-01-12T16:13:23Z
```

# ^ doesn't match first line when BOM is present with encoding 'auto'

---

_@roblourens_

[bom.txt](https://github.com/BurntSushi/ripgrep/files/2756332/bom.txt)

- Download bom.txt which has a utf8 BOM
- `rg '^test123'` -> 1 result (should be 2 results)
- `rg '^test123' --encoding 'utf8'` -> 2 results

---

_Label `bug` added by @BurntSushi on 2019-01-14 18:09_

---

_Closed by @BurntSushi on 2019-01-25 22:19_

---
