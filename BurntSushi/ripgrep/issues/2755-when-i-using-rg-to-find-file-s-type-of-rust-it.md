```yaml
number: 2755
title: "When i using rg to find file's type of rust, it doesn't work."
type: issue
state: closed
author: charryong
labels:
  - invalid
assignees: []
created_at: 2024-03-12T03:16:45Z
updated_at: 2024-03-12T03:23:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2755
synced_at: 2026-01-12T16:13:24Z
```

# When i using rg to find file's type of rust, it doesn't work.

---

_@charryong_

I'm using this version. **"ripgrep 14.1.0 (rev e50df40a19)"**

rg -t rs hello
rg: unrecognized file type: rs

---

_Comment by @BurntSushi on 2024-03-12 03:23_

Please consult `rg --type-list`. The error message is telling you what the problem is. `rs` isn't a recognized file type.

---

_Closed by @BurntSushi on 2024-03-12 03:23_

---

_Label `invalid` added by @BurntSushi on 2024-03-12 03:23_

---
