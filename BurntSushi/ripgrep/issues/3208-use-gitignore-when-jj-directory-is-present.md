```yaml
number: 3208
title: Use .gitignore when .jj directory is present
type: issue
state: closed
author: gbirke
labels:
  - duplicate
assignees: []
created_at: 2025-10-24T07:56:35Z
updated_at: 2025-10-24T12:01:36Z
url: https://github.com/BurntSushi/ripgrep/issues/3208
synced_at: 2026-01-12T16:13:25Z
```

# Use .gitignore when .jj directory is present

---

_@gbirke_

#### Describe your feature request

I'm currently using [Jujutsu VCS](https://jj-vcs.github.io/), a Git-compatible VCS that uses `.gitignore` files. I use it without its co-location feature (which places a `.git` directory in the project root). Instead, I only have a `.jj` directory in my project root. It would be nice if `rg` could look for both `.git` and `.jj` directories to determine if it should observe the `.gitignore` file.

The workaround, using `--norequire-git`, is a bit cumbershome.



---

_Comment by @BurntSushi on 2025-10-24 12:01_

Already done in https://github.com/BurntSushi/ripgrep/pull/2842

---

_Closed by @BurntSushi on 2025-10-24 12:01_

---

_Label `duplicate` added by @BurntSushi on 2025-10-24 12:01_

---
