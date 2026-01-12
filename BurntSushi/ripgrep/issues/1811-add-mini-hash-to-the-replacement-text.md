```yaml
number: 1811
title: Add mini hash to the replacement text
type: issue
state: closed
author: bshakur8
labels:
  - wontfix
assignees: []
created_at: 2021-03-01T11:35:22Z
updated_at: 2021-03-01T11:41:54Z
url: https://github.com/BurntSushi/ripgrep/issues/1811
synced_at: 2026-01-12T16:13:24Z
```

# Add mini hash to the replacement text

---

_@bshakur8_

It would be useful to add and mini-hash (identifier) along with each replacement text.

For example: I would like to replace all IPv4 addresses with `<IPv4>` but I would like to give it another identifier to indicate that these two IPs are the same. So they will be hashed to the same string and will be replaced by the same replacement string, i.e IP=10.0.0.1 with be replaced by `<IPv4-a3gb1>`. So in all occurrences of IP 10.0.0.1, it will be replaced **always** by `<IPv4-a3gb1>`
 
It will give a powerful obfuscation over multiple files on different servers.

---

_Comment by @BurntSushi on 2021-03-01 11:41_

Thanks for the suggestion, but I'm not planning on enhancing ripgrep's replacement facilities. It provides something minimally useful, but it is not a general purpose replacement tool. You'll need something else for that.

---

_Closed by @BurntSushi on 2021-03-01 11:41_

---

_Label `wontfix` added by @BurntSushi on 2021-03-01 11:41_

---
