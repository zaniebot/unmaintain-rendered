```yaml
number: 3192
title: add Cursor hyperlink alias
type: pull_request
state: merged
author: enochchau
labels: []
assignees: []
merged: true
base: master
head: ec-cursor-hyperlink-format
created_at: 2025-10-17T18:45:02Z
updated_at: 2025-10-17T19:03:54Z
url: https://github.com/BurntSushi/ripgrep/pull/3192
synced_at: 2026-01-12T18:23:15Z
```

# add Cursor hyperlink alias

---

_@enochchau_

Add hyperlink alias for Cursor. It works the same way as the other VSCode forks.
I have the following code in my zshrc but I would like to add the alias officially.
```sh
if [[ "$TERM_PROGRAM" == 'vscode' ]]; then
  alias rg='rg --hyperlink-format="cursor://file{path}:{line}:{column}"'
fi
```

---

_Renamed from "add cursor hyperlink alias" to "add Cursor hyperlink alias" by @enochchau on 2025-10-17 18:45_

---

_Review comment by @BurntSushi on `crates/printer/src/hyperlink/aliases.rs`:67 on 2025-10-17 18:49_

This should be at the top of the list. Note the failing test and the comments above.

---

_@BurntSushi reviewed on 2025-10-17 18:49_

---

_@BurntSushi approved on 2025-10-17 18:58_

Thanks!

---

_Merged by @BurntSushi on 2025-10-17 18:59_

---

_Closed by @BurntSushi on 2025-10-17 18:59_

---

_Branch deleted on 2025-10-17 19:03_

---
