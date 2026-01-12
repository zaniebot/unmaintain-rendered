```yaml
number: 4
title: stream results when feasible
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2016-09-13T11:49:49Z
updated_at: 2016-09-14T01:12:38Z
url: https://github.com/BurntSushi/ripgrep/issues/4
synced_at: 2026-01-12T18:23:11Z
```

# stream results when feasible

---

_@BurntSushi_

Currently, all search results are written to an intermediate buffer _in memory_ before being actually emitted to `stdout`. This is done to permit more efficient parallelism when searching. That is, only one thread can be writing to `stdout` at any point in time, but multiple threads can write to their own thread local memory buffer.

This does have undesirable end user consequences:
1. It can result in high memory usage when the number of search results is high.
2. When searching a single file, no output is seen until the search is complete.

We should be able to do quite a bit to fix these issues:
1. If a single file is given to search, then don't try any parallelism and make searching write to `stdout` directly.
2. If `--threads 1` is given, then do (1) regardless of the number of inputs.


---

_Label `enhancement` added by @BurntSushi on 2016-09-13 11:49_

---

_Closed by @BurntSushi on 2016-09-14 01:11_

---

_Comment by @BurntSushi on 2016-09-14 01:12_

This is done, but I opted to only do it for searching single files or stdin. Setting `--threads 1` will still use the intermediate buffer.


---
