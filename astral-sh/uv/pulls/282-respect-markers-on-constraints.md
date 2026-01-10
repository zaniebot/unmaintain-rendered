```yaml
number: 282
title: Respect markers on constraints
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/python-constraint-marker
created_at: 2023-11-01T21:49:12Z
updated_at: 2023-11-02T01:20:33Z
url: https://github.com/astral-sh/uv/pull/282
synced_at: 2026-01-10T15:50:28Z
```

# Respect markers on constraints

---

_Pull request opened by @zanieb on 2023-11-01 21:49_

Closes #252 

---

_@charliermarsh reviewed on 2023-11-01 21:57_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:257 on 2023-11-01 21:57_

Can we add a test case that succeeds, e.g., add a dependency for `python_version <= 3.7` and ensure it's not respected?

---

_@charliermarsh reviewed on 2023-11-01 21:59_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:487 on 2023-11-01 21:59_

Do you mind removing this while you're here? I think I added this accidentally.

---

_@charliermarsh reviewed on 2023-11-01 22:00_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:490 on 2023-11-01 22:00_

This looks right to me.

---

_@zanieb reviewed on 2023-11-01 22:33_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:257 on 2023-11-01 22:33_

Yeah I can. I don't know about this test case it's not really doing what I want.

---

_@zanieb reviewed on 2023-11-01 22:33_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver.rs`:487 on 2023-11-01 22:33_

👍 

---

_@zanieb reviewed on 2023-11-01 22:44_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:257 on 2023-11-01 22:44_

I used a simpler dependency tree for this test, much better now.

---

_Marked ready for review by @zanieb on 2023-11-01 22:44_

---

_@charliermarsh approved on 2023-11-01 23:33_

---

_Merged by @zanieb on 2023-11-02 01:20_

---

_Closed by @zanieb on 2023-11-02 01:20_

---

_Branch deleted on 2023-11-02 01:20_

---
