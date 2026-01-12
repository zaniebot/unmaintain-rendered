```yaml
number: 2016
title: "fix: make query method of interpreter public"
type: pull_request
state: merged
author: tdejager
labels: []
assignees: []
merged: true
base: main
head: fix/interpreter-query-public
created_at: 2024-02-27T14:31:34Z
updated_at: 2024-02-27T15:03:37Z
url: https://github.com/astral-sh/uv/pull/2016
synced_at: 2026-01-12T16:04:50Z
```

# fix: make query method of interpreter public

---

_@tdejager_

## Summary

Made the `query` method public again, as I believe this currently the only way to query a intepreter with a custom location.

Closes: #2015 

---

_@charliermarsh approved on 2024-02-27 14:39_

---

_Merged by @charliermarsh on 2024-02-27 14:39_

---

_Closed by @charliermarsh on 2024-02-27 14:39_

---

_@MichaReiser reviewed on 2024-02-27 14:44_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/interpreter.rs`:38 on 2024-02-27 14:44_

Nit: It might be worth adding a comment here why the function is public. I could otherwise see myself to make it `pub(crate)` in the future when I don't see any public usages or we should have write up a document listing the APIs that are considered *stable*/  used downstream

---

_@tdejager reviewed on 2024-02-27 15:03_

---

_Review comment by @tdejager on `crates/uv-interpreter/src/interpreter.rs`:38 on 2024-02-27 15:03_

Sounds like a good idea, if I encounter it again, I'll make sure to include it in the comment :)

---
