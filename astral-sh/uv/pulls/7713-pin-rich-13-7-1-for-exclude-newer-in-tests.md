```yaml
number: 7713
title: Pin rich==13.7.1 for --exclude-newer in tests
type: pull_request
state: merged
author: manzt
labels: []
assignees: []
merged: true
base: main
head: manzt/fix-rich-pin-in-remote-script
created_at: 2024-09-26T15:38:46Z
updated_at: 2024-09-26T15:56:45Z
url: https://github.com/astral-sh/uv/pull/7713
synced_at: 2026-01-10T12:53:53Z
```

# Pin rich==13.7.1 for --exclude-newer in tests

---

_Pull request opened by @manzt on 2024-09-26 15:38_

Fixes broken test in #6375

Tested here with:

```sh
cargo run run --exclude-newer=2024-03-25T00:00:00Z scripts/uv-run-remote-script-test.py CI
```


---

_@BurntSushi approved on 2024-09-26 15:54_

---

_Merged by @BurntSushi on 2024-09-26 15:54_

---

_Closed by @BurntSushi on 2024-09-26 15:54_

---

_Branch deleted on 2024-09-26 15:56_

---
