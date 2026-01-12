```yaml
number: 4951
title: Warn if tool binary directory is not on path
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2024-07-10T02:01:19Z
updated_at: 2024-07-10T15:24:18Z
url: https://github.com/astral-sh/uv/pull/4951
synced_at: 2026-01-12T16:06:33Z
```

# Warn if tool binary directory is not on path

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/4671.

## Test Plan

```
❯ XDG_BIN_HOME="/Users/crmarsh/workspace/uv/foo bar" cargo run tool install black --force
Installed 2 executables: black, blackd
warning: `/Users/crmarsh/workspace/uv/foo bar` is not on your PATH. To use installed tools, run:
  export PATH="/Users/crmarsh/workspace/uv/foo bar:$PATH"
```

---

_Label `error messages` added by @charliermarsh on 2024-07-10 02:01_

---

_Label `preview` added by @charliermarsh on 2024-07-10 02:01_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-10 02:01_

---

_@charliermarsh reviewed on 2024-07-10 02:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:397 on 2024-07-10 02:02_

Obviously not all tested, some ChatGPT help here...

---

_@charliermarsh reviewed on 2024-07-10 02:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:387 on 2024-07-10 02:02_

Couldn't find a short invocation for this, it looks like you have to modify a file and source it.

---

_Review requested from @konstin by @charliermarsh on 2024-07-10 02:10_

---

_@konstin approved on 2024-07-10 07:42_

---

_Merged by @charliermarsh on 2024-07-10 15:24_

---

_Closed by @charliermarsh on 2024-07-10 15:24_

---

_Branch deleted on 2024-07-10 15:24_

---
