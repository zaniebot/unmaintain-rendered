```yaml
number: 8472
title: "Support `--with-editable` in `uv tool install`"
type: pull_request
state: merged
author: blueraft
labels: []
assignees: []
merged: true
base: main
head: editable-tool-install
created_at: 2024-10-22T17:43:39Z
updated_at: 2024-10-23T00:06:34Z
url: https://github.com/astral-sh/uv/pull/8472
synced_at: 2026-01-12T16:08:20Z
```

# Support `--with-editable` in `uv tool install`

---

_@blueraft_

## Summary

Closes https://github.com/astral-sh/uv/issues/7528

## Test Plan

`cargo test`


---

_@zanieb approved on 2024-10-22 18:25_

Thanks!

---

_@zanieb reviewed on 2024-10-22 18:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:176 on 2024-10-22 18:26_

I think you need 
```suggestion
    let context = TestContext::new("3.12").with_filtered_counts().with_filtered_exe_suffix();
```

---

_@blueraft reviewed on 2024-10-22 18:29_

---

_Review comment by @blueraft on `crates/uv/tests/it/tool_install.rs`:176 on 2024-10-22 18:29_

thanks!

---

_Merged by @charliermarsh on 2024-10-23 00:06_

---

_Closed by @charliermarsh on 2024-10-23 00:06_

---
