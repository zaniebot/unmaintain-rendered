```yaml
number: 2561
title: "ignore/types: add TCC files"
type: pull_request
state: closed
author: mataha
labels: []
assignees: []
base: master
head: feature/ignore/types/tcc
created_at: 2023-07-18T17:16:59Z
updated_at: 2023-07-19T00:18:04Z
url: https://github.com/BurntSushi/ripgrep/pull/2561
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add TCC files

---

_@mataha_

This PR adds file types for TCC, a `cmd.exe` extension.

Extensions are the same as for `cmd` itself + `*.btm`[1].

[1]: https://jpsoft.com/help/batchtype.htm

---

_Comment by @BurntSushi on 2023-07-18 17:31_

TCC looks like proprietary software to me. Which is fine, but I generally only include FOSS stuff in ripgrep's default set of types. ripgrep's file type list is extensible, so you can maintain your own rules.

---

_Comment by @mataha on 2023-07-18 18:18_

That's fair (though I believe the runtime is free for unlimited use).

---

_Closed by @mataha on 2023-07-18 18:18_

---

_Branch deleted on 2023-07-19 00:18_

---
