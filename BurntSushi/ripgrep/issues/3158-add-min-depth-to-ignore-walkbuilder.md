```yaml
number: 3158
title: "Add `min_depth` to `ignore::WalkBuilder`"
type: issue
state: closed
author: jalil-salame
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-09-24T09:47:37Z
updated_at: 2025-10-05T14:05:28Z
url: https://github.com/BurntSushi/ripgrep/issues/3158
synced_at: 2026-01-12T16:13:25Z
```

# Add `min_depth` to `ignore::WalkBuilder`

---

_@jalil-salame_

#### Describe your feature request

I am migrating from `walkdir` to `ignore` so I can support `.dockerignore` files, and we use the `walkdir::WalkDir::min_depth` method which has no equivalent in `ignore::WalkBuilder`.

I'd be willing to add the method.

Also, thanks for all the great crates!


---

_Comment by @BurntSushi on 2025-09-24 13:22_

A patch with tests is welcome.

Note also that `min_depth` is mostly just a convenience. You can always filter on [`DirEntry::depth`](https://docs.rs/ignore/latest/ignore/struct.DirEntry.html#method.depth) yourself.

---

_Label `enhancement` added by @BurntSushi on 2025-09-24 13:23_

---

_Label `help wanted` added by @BurntSushi on 2025-09-24 13:23_

---

_Comment by @jalil-salame on 2025-09-24 14:00_

Yup! I had to look at the code to ensure that `min_depth` and `DirEntry::depth` would do the same thing, so that's why I raised the issue.

---

_Closed by @BurntSushi on 2025-10-05 14:05_

---
