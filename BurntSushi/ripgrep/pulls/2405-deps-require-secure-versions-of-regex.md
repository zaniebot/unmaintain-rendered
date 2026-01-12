```yaml
number: 2405
title: "deps: require secure versions of regex"
type: pull_request
state: closed
author: robjtede
labels: []
assignees: []
base: master
head: secure-deps
created_at: 2023-01-27T15:50:45Z
updated_at: 2023-02-07T18:45:28Z
url: https://github.com/BurntSushi/ripgrep/pull/2405
synced_at: 2026-01-12T18:23:14Z
```

# deps: require secure versions of regex

---

_@robjtede_

addresses warnings seen here: https://deps.rs/repo/github/BurntSushi/ripgrep#vulnerabilities

---

_Comment by @BurntSushi on 2023-01-27 15:57_

The lock file already has a more up to date `regex` crate used.

Both of those "vulnerabilities" are false positives in the context of ripgrep. The `thread_local` vulnerability doesn't impact ripgrep because it doesn't use the iteration APIs. The `regex` vulnerability is ReDoS which doesn't make sense in the context of ripgrep since it's a command line tool.

I'm sure these versions will get updated at some point, but I don't see any compelling reason to do that now.

---

_Closed by @BurntSushi on 2023-01-27 15:57_

---

_Branch deleted on 2023-02-07 18:42_

---

_Comment by @robjtede on 2023-02-07 18:43_

Yea, you're right. We should change deps.rs to [read lockfiles](https://github.com/deps-rs/deps.rs/issues/26) so these issues are not flagged on application projects.

---
