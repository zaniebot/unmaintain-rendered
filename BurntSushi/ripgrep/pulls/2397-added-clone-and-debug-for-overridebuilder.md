```yaml
number: 2397
title: Added Clone and Debug for OverrideBuilder
type: pull_request
state: merged
author: vallentin
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2023-01-15T12:44:39Z
updated_at: 2023-01-15T13:21:29Z
url: https://github.com/BurntSushi/ripgrep/pull/2397
synced_at: 2026-01-12T18:23:14Z
```

# Added Clone and Debug for OverrideBuilder

---

_@vallentin_

I couldn't find a reason for this being intentional. `Clone` and `Debug` is also already implemented for `Override`, `Gitignore`, and `GitignoreBuilder`. But not for `OverrideBuilder`.

In my own project, I have a custom builder containing an `OverrideBuilder`, and would like to be able to add `Clone` to my builder.


---

_Merged by @BurntSushi on 2023-01-15 13:16_

---

_Closed by @BurntSushi on 2023-01-15 13:16_

---

_Comment by @BurntSushi on 2023-01-15 13:21_

This is on crates.io in `ignore 0.4.20`.

---
