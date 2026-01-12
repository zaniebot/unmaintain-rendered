```yaml
number: 16778
title: "Fix `uv init` author serialization via `toml_edit` inline tables"
type: pull_request
state: merged
author: terror
labels:
  - bug
assignees: []
merged: true
base: main
head: author-toml-fmt
created_at: 2025-11-19T17:29:11Z
updated_at: 2025-11-20T19:10:21Z
url: https://github.com/astral-sh/uv/pull/16778
synced_at: 2026-01-12T16:12:26Z
```

# Fix `uv init` author serialization via `toml_edit` inline tables

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16765

---

_@woodruffw reviewed on 2025-11-19 17:44_

---

_Review comment by @woodruffw on `crates/uv/src/commands/project/init.rs`:952 on 2025-11-19 17:44_

Curious: do we want `get_or_insert` here, or just `insert`? To my understanding we're building the `pyproject.toml` from complete scratch, so there's no "get" path to take anyways.

---

_@terror reviewed on 2025-11-19 17:49_

---

_Review comment by @terror on `crates/uv/src/commands/project/init.rs`:952 on 2025-11-19 17:49_

Yeah agreed, [get_or_insert](https://docs.rs/toml_edit/latest/toml_edit/struct.InlineTable.html#method.get_or_insert) is slightly nicer since it can handle value conversion, but we don't use its functionality here.

---

_@woodruffw reviewed on 2025-11-19 17:53_

---

_Review comment by @woodruffw on `crates/uv/src/commands/project/init.rs`:952 on 2025-11-19 17:53_

Thanks! Yeah, kinda funky that `insert` doesn't have `V: Into<Value>` like `get_or_insert` does. 

---

_@woodruffw approved on 2025-11-19 17:53_

---

_Review comment by @terror on `crates/uv/src/commands/project/init.rs`:952 on 2025-11-19 18:01_

Weird for sure, I put up a PR here https://github.com/toml-rs/toml/pull/1069 🤷 

---

_@terror reviewed on 2025-11-19 18:01_

---

_Label `bug` added by @konstin on 2025-11-20 19:09_

---

_Merged by @woodruffw on 2025-11-20 19:10_

---

_Closed by @woodruffw on 2025-11-20 19:10_

---

_Comment by @woodruffw on 2025-11-20 19:10_

Thanks @terror!

---
