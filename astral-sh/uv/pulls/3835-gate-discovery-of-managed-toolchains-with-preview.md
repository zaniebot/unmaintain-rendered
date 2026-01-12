```yaml
number: 3835
title: Gate discovery of managed toolchains with preview
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-preview-mode
created_at: 2024-05-25T14:05:07Z
updated_at: 2024-05-27T04:05:20Z
url: https://github.com/astral-sh/uv/pull/3835
synced_at: 2026-01-12T16:05:53Z
```

# Gate discovery of managed toolchains with preview

---

_@zanieb_

Prepares for merge of https://github.com/astral-sh/uv/pull/3797, gating managed toolchain discovery with the preview flag to lower risk of releasing.

e.g.

```
❯ cargo dev -q fetch-python         
❯ cargo run -q -- venv --python 3.11
Using Python 3.11.9 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
❯ cargo run -q -- venv --preview --python 3.11
Using Python 3.11.7 interpreter at: /Users/zb/Library/Application Support/uv/toolchains/cpython-3.11.7-macos-aarch64-none/install/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

We'll add automatic fetching of managed interpreters later.

---

_Label `preview` added by @zanieb on 2024-05-25 14:05_

---

_Marked ready for review by @zanieb on 2024-05-25 14:08_

---

_Review requested from @charliermarsh by @zanieb on 2024-05-25 14:10_

---

_@zanieb reviewed on 2024-05-25 14:11_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:1299 on 2024-05-25 14:11_

I'll probably rethink this display in a future change.

---

_@zanieb reviewed on 2024-05-25 17:06_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:416 on 2024-05-25 17:06_

We turn on preview so we discover managed toolchains when finding the Python versions requested by tests.

---

_@charliermarsh approved on 2024-05-27 03:57_

Looks reasonable to me.

---

_Merged by @charliermarsh on 2024-05-27 04:05_

---

_Closed by @charliermarsh on 2024-05-27 04:05_

---

_Branch deleted on 2024-05-27 04:05_

---
