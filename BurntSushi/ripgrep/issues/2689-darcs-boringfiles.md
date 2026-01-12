```yaml
number: 2689
title: darcs boringfiles
type: issue
state: closed
author: toastal
labels:
  - wontfix
assignees: []
created_at: 2023-12-14T09:38:36Z
updated_at: 2023-12-14T14:17:11Z
url: https://github.com/BurntSushi/ripgrep/issues/2689
synced_at: 2026-01-12T16:13:24Z
```

# darcs boringfiles

---

_@toastal_

#### Describe your feature request

Like Git + gitignore files, darcs supports boringfiles that users would like to skip over. There is a default set, but also can be overridden by users on a global + per-project bases using `darcs setpref boringfile` which saves the setting in the `_darcs/prefs/prefs` file (typically by convention this file is `$PROJ_ROOT/.boring`, but doesn’t have to be).

---

_Comment by @BurntSushi on 2023-12-14 14:17_

Sorry, but darcs isn't widely used enough to justify the work to add this IMO. ripgrep doesn't even have Mercurial support.

Darcs users will likely need to find some way to duplicate their ignore files. One for Darcs and another, i.e., `.rgignore`, for ripgrep.

---

_Closed by @BurntSushi on 2023-12-14 14:17_

---

_Label `wontfix` added by @BurntSushi on 2023-12-14 14:17_

---
