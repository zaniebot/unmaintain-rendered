```yaml
number: 2918
title: Add --histogram=NUM argument
type: pull_request
state: closed
author: Cabbache
labels: []
assignees: []
base: master
head: master
created_at: 2024-10-23T21:37:12Z
updated_at: 2025-08-17T13:32:21Z
url: https://github.com/BurntSushi/ripgrep/pull/2918
synced_at: 2026-01-12T18:23:14Z
```

# Add --histogram=NUM argument

---

_@Cabbache_

With this output mode, `rg` will print a histogram of the number of matches within certain ranges of byte offsets. The width of the ranges is the bin size which is specified in the argument.

---

_Comment by @BurntSushi on 2025-08-17 13:32_

Thanks for working on this. It looks neat. But I think this is too niche of a use case to have in ripgrep proper. And it seems likely to be a feature that begets more requests in terms of customizing how it works. It seems like you should be able to calculate this in an external tool by reading ripgrep's JSON output.

---

_Closed by @BurntSushi on 2025-08-17 13:32_

---
