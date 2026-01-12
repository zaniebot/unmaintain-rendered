```yaml
number: 2971
title: README.md screenshot broken (references missing external image)
type: issue
state: closed
author: mbrukman
labels: []
assignees: []
created_at: 2025-01-13T16:25:29Z
updated_at: 2025-01-28T01:54:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2971
synced_at: 2026-01-12T16:13:25Z
```

# README.md screenshot broken (references missing external image)

---

_@mbrukman_

The [`README.md`](https://github.com/BurntSushi/ripgrep/blob/d6b59feff890f038acde7d5e151995d8aec1e107/README.md?plain=1#L34-L36) features a screenshot:

https://github.com/BurntSushi/ripgrep/blob/d6b59feff890f038acde7d5e151995d8aec1e107/README.md?plain=1#L34-L36

However, the URL in question (https://burntsushi.net/stuff/ripgrep1.png) returns 404 not found, leading to a broken image.

If you still have this image, it would be helpful to include it in the repo itself as documentation and reference it using a local URL, or perhaps restore or recreate it so that it can be included in the docs.

---

_Comment by @BurntSushi on 2025-01-13 16:34_

Yeah I think I flubbed this when I moved my web site to a new server. But I took backups and I've restored this image.

I agree that it would be nicer to include the image in the repository, but I think the immediate problem is fixed.

---

_Closed by @BurntSushi on 2025-01-13 16:34_

---

_Comment by @mbrukman on 2025-01-28 01:54_

@BurntSushi — thanks for fixing this so quickly, and thanks for ripgrep!

---
