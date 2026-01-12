```yaml
number: 3850
title: "Make `CallPath` its own struct"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/call-path-struct
created_at: 2023-04-01T16:07:10Z
updated_at: 2023-04-23T04:00:11Z
url: https://github.com/astral-sh/ruff/pull/3850
synced_at: 2026-01-12T04:28:19Z
```

# Make `CallPath` its own struct

---

_Pull request opened by @charliermarsh on 2023-04-01 16:07_

## Summary

I haven't gone about changing all the usages yet, more looking for feedback here, but this PR rewrites `CallPath` from `pub type CallPath<'a> = smallvec::SmallVec<[&'a str; 8]>` to `pub struct CallPath<'a>(smallvec::SmallVec<[&'a str; 8]>)`, which allows us to implement various methods on the struct directly rather than implementing standalone utilities.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-04-01 16:07_

---

_Closed by @charliermarsh on 2023-04-23 04:00_

---
