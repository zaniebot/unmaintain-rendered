```yaml
number: 2583
title: json format for rg --files 
type: issue
state: closed
author: taoxinyi
labels:
  - wontfix
assignees: []
created_at: 2023-08-15T17:55:52Z
updated_at: 2023-08-15T18:35:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2583
synced_at: 2026-01-12T16:13:24Z
```

# json format for rg --files 

---

_@taoxinyi_

Encountered a special file today (which contains new line \n in the filename) and it makes difficult to parse the output of `rg --files`. It would be nice to have a json format for `rg --files`. Assuming the filename is `a.txt \n` then the output of `rg --files` will be
```
a.txt

b.txt
```

---

_Comment by @BurntSushi on 2023-08-15 17:58_

I'm not adding a JSON format for this, sorry. It isn't worth it IMO.

You might consider trying the `-0/--null` flag instead, which will emit file paths delimited by NUL terminators instead of line terminators.

---

_Closed by @BurntSushi on 2023-08-15 17:58_

---

_Label `wontfix` added by @BurntSushi on 2023-08-15 17:58_

---

_Comment by @taoxinyi on 2023-08-15 18:35_

oh I don't know this flag before. Thank you that works perfectly!

---
