```yaml
number: 2833
title: Optimize character escaping.
type: pull_request
state: merged
author: TDecking
labels: []
assignees: []
merged: true
base: master
head: optimize-escape
created_at: 2024-06-05T13:46:33Z
updated_at: 2024-06-05T14:27:17Z
url: https://github.com/BurntSushi/ripgrep/pull/2833
synced_at: 2026-01-12T18:23:14Z
```

# Optimize character escaping.

---

_@TDecking_

Rewrites the `char_to_escaped_literal` and `bytes_to_escaped_literal` functions in a way that minimizes heap allocations.
After this, the resulting string is the only allocation remaining.

---

_@BurntSushi approved on 2024-06-05 13:54_

Thanks!

---

_Merged by @BurntSushi on 2024-06-05 13:56_

---

_Closed by @BurntSushi on 2024-06-05 13:56_

---

_Branch deleted on 2024-06-05 14:27_

---
