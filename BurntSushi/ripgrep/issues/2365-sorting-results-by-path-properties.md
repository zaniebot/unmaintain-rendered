```yaml
number: 2365
title: sorting results by path properties
type: issue
state: closed
author: nguyenvukhang
labels:
  - duplicate
assignees: []
created_at: 2022-11-29T02:35:09Z
updated_at: 2022-11-29T19:09:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2365
synced_at: 2026-01-12T16:13:24Z
```

# sorting results by path properties

---

_@nguyenvukhang_

As outlined in https://github.com/BurntSushi/ripgrep/issues/2243, results are incorrectly sorted by file stats (modified/created/accessed time).

One approach is to make the recursive directory traversal smarter. For example, when sorting by latest modified, the parent directory will be assigned the latest modified time of all its children, rather than the direct result of calling `stat` on it.

Another approach will be to sort the results after all search results have been matched and collected. This will likely collecting all results into memory, tagging each result with a key based on the sorting criteria, and sorting the results by this key.

---

_Comment by @BurntSushi on 2022-11-29 19:09_

Let's try to keep this in one issue instead of creating many for the same problem. I moved this comment to [here](https://github.com/BurntSushi/ripgrep/issues/2243#issuecomment-1331166864).

---

_Closed by @BurntSushi on 2022-11-29 19:09_

---

_Label `duplicate` added by @BurntSushi on 2022-11-29 19:09_

---
