```yaml
number: 3162
title: "Implement min depth on `ignore`"
type: pull_request
state: merged
author: AlvaroParker
labels: []
assignees: []
merged: true
base: master
head: min-depth-on-walk
created_at: 2025-09-28T22:36:26Z
updated_at: 2025-10-05T14:07:36Z
url: https://github.com/BurntSushi/ripgrep/pull/3162
synced_at: 2026-01-12T18:23:15Z
```

# Implement min depth on `ignore`

---

_@AlvaroParker_

Closes #3158 

This replicates the behavior of `walkdir`, where passing `.min_depth(1)`, `.min_depth(0)` or no `min_depth` will have the same behavior.

Passing `min_depth(Some(2)` to the builder to a directory that looks like this:

```
.
├── a
│   ├── b
│   │   ├── c
│   │   │   └── foo
│   │   └── foo
│   └── foo
└── foo
```

Will returns something like this:

```
["a/b", "a/b/c", "a/b/c/foo", "a/b/foo", "a/foo"]
```

While passing a `min_depth` of `0`, `1` or `None`: 

```
["a", "a/b", "a/b/c", "foo", "a/foo", "a/b/foo", "a/b/c/foo"]
```

If the user passes a `max_depth` < `min_depth` then we set `max_depth = min_depth` as `walkdir` does. 

---

_Merged by @BurntSushi on 2025-10-05 14:05_

---

_Closed by @BurntSushi on 2025-10-05 14:05_

---

_Comment by @jalil-salame on 2025-10-05 14:07_

Thanks for doing this c:

---
