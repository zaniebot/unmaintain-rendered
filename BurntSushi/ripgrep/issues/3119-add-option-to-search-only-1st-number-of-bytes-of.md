```yaml
number: 3119
title: "Add option to search only 1st <number of bytes> of each file."
type: issue
state: closed
author: SaintNick2023
labels:
  - duplicate
assignees: []
created_at: 2025-08-05T14:04:44Z
updated_at: 2025-08-05T14:09:57Z
url: https://github.com/BurntSushi/ripgrep/issues/3119
synced_at: 2026-01-12T16:13:25Z
```

# Add option to search only 1st <number of bytes> of each file.

---

_@SaintNick2023_

It would be great if we could have an option to fetch/read and search only 1st [number of bytes] of each file. E.g.

`rg --max-bytes 1048576`

I'm sure there will plenty of use cases for that, especially combined together with --max-count 1 it would give a huge performance boost over repo's of many large files where the searchstring is in a header block.

---

_Closed by @BurntSushi on 2025-08-05 14:09_

---

_Label `duplicate` added by @BurntSushi on 2025-08-05 14:09_

---
