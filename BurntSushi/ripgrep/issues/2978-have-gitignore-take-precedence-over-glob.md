```yaml
number: 2978
title: Have gitignore take precedence over glob
type: issue
state: closed
author: zoriya
labels:
  - wontfix
assignees: []
created_at: 2025-01-24T11:17:58Z
updated_at: 2025-01-24T11:43:43Z
url: https://github.com/BurntSushi/ripgrep/issues/2978
synced_at: 2026-01-12T16:13:25Z
```

# Have gitignore take precedence over glob

---

_@zoriya_

#### Describe your feature request

When adding `--glob` to the args items from `.gitignore` shows. I know this is by-design and `glob` enables back those items but I would like a way to combine gitignore with glob. 
Maybe `--glob` could override the default `.gitignore` while `--gitignore --glob` could ignore both git-ignored files & file that don't match glob?

---

_Comment by @BurntSushi on 2025-01-24 11:43_

No, I don't see this changing. The system is already hideously complicated, and your proposed rule looks extremely subtle to me.

---

_Closed by @BurntSushi on 2025-01-24 11:43_

---

_Label `wontfix` added by @BurntSushi on 2025-01-24 11:43_

---
