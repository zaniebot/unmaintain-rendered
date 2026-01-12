```yaml
number: 2577
title: "globset: use non-capture groups in regex transform"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/globset-non-capture
created_at: 2023-08-04T18:10:53Z
updated_at: 2023-08-05T13:34:07Z
url: https://github.com/BurntSushi/ripgrep/pull/2577
synced_at: 2026-01-12T18:23:14Z
```

# globset: use non-capture groups in regex transform

---

_@BurntSushi_

We currently implement globs by converting them to regexes, and in doing so, sometimes use grouping. In all but one case, we used non-capturing groups. But for alternations, we used capturing groups, which was likely just an oversight. We don't make use of capture groups at all, and while they usually don't have any overhead, they lead to weird cases like this one: https://github.com/rust-lang/regex/issues/1059

That particular issue is also a bug in the regex crate itself, which is fixed in https://github.com/rust-lang/regex/pull/1062. Note though that the bug fix in the regex crate is required. Even with this patch to globset, memory usage is reduced (by about half in rust-lang/regex#1059) but is not returned to where it was prior to the regex 1.9 release.

---

_Merged by @BurntSushi on 2023-08-05 13:33_

---

_Closed by @BurntSushi on 2023-08-05 13:33_

---

_Branch deleted on 2023-08-05 13:34_

---
