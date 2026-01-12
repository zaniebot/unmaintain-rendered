```yaml
number: 2913
title: "[docs] Indicate only one filter can be applied for `ignore::WalkBuilder`"
type: issue
state: closed
author: bicarlsen
labels: []
assignees: []
created_at: 2024-10-21T06:12:21Z
updated_at: 2025-09-23T01:49:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2913
synced_at: 2026-01-12T16:13:25Z
```

# [docs] Indicate only one filter can be applied for `ignore::WalkBuilder`

---

_@bicarlsen_

#### Describe your feature request
In the [docs for `ignore::WalkBuilder::filter_entry`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.filter_entry) it could be useful to note that only one filter can be used, and calling `filter_entry` multiple times will overwrite the previous filter. 

I had assumed this call was composable such as `Iterator::filter` is.

---

_Closed by @BurntSushi on 2025-09-23 01:49_

---
