```yaml
number: 3123
title: Fix broken command in documentation
type: pull_request
state: closed
author: Propfend
labels: []
assignees: []
base: master
head: fix_doc_broken_link
created_at: 2025-08-12T18:43:19Z
updated_at: 2025-08-19T20:44:49Z
url: https://github.com/BurntSushi/ripgrep/pull/3123
synced_at: 2026-01-12T18:23:15Z
```

# Fix broken command in documentation

---

_@Propfend_

The documentation uses the `ripgrep_14.1.1-1_amd64.deb` file from `https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep_14.1.1-1_amd64.deb` URL. But the name of file is `ripgrep_14.1.0-1_amd64` (not 14.1.1-1) as you can see [here](https://github.com/BurntSushi/ripgrep/releases/tag/14.1.0). This PR fixes this [issue](https://github.com/BurntSushi/ripgrep/issues/3083) By correcting name in second command.

---

_Closed by @Propfend on 2025-08-12 18:50_

---

_Renamed from "Fix broken link in documentation" to "Fix broken command in documentation" by @Propfend on 2025-08-12 20:52_

---

_Reopened by @Propfend on 2025-08-12 20:53_

---

_Comment by @BurntSushi on 2025-08-19 20:44_

Duplicate of #3058

---

_Closed by @BurntSushi on 2025-08-19 20:44_

---
