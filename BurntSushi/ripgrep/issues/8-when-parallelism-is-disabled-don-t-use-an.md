```yaml
number: 8
title: "when parallelism is disabled, don't use an intermediate buffer"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2016-09-19T15:23:08Z
updated_at: 2016-09-26T01:30:13Z
url: https://github.com/BurntSushi/ripgrep/issues/8
synced_at: 2026-01-12T18:23:11Z
```

# when parallelism is disabled, don't use an intermediate buffer

---

_@BurntSushi_

When `--threads 1` is given to `ripgrep`, it should not write results to an intermediate buffer. In particular, if every search is serialized (as it is when `--threads 1` is given), then no synchronization is needed to print to `stdout`, and therefore, no intermediate buffer is needed.

This should be somewhat straight-forward, since the searcher is generic over a `Printer<W: Terminal>`.

In #4, I ended up doing this, but only when there was a single file (or `stdin`) to be searched.


---

_Label `bug` added by @BurntSushi on 2016-09-19 15:23_

---

_Closed by @BurntSushi on 2016-09-26 01:30_

---
