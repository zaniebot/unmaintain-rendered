```yaml
number: 6124
title: Allow displaying the derivation tree
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - tracing
assignees: []
merged: true
base: main
head: zb/show-tree
created_at: 2024-08-15T17:38:22Z
updated_at: 2024-08-16T14:25:27Z
url: https://github.com/astral-sh/uv/pull/6124
synced_at: 2026-01-10T13:09:50Z
```

# Allow displaying the derivation tree

---

_Pull request opened by @zanieb on 2024-08-15 17:38_

I need this for debugging error messages.

I used an environment variable instead of a trace log so you can do `UV_INTERNAL__SHOW_DERIVATION_TREE=1` and run a test to see the tree in the test snapshot without further changes.

e.g.

```rust
    // Resolving should fail.
    uv_snapshot!(context.filters(), context.lock().arg("--preview").current_dir(&workspace), @r###"
    success: false
    exit_code: 1
    ----- stdout -----
    UV_INTERNAL__SHOW_DERIVATION_TREE
      root==0a0.dev0 depends on foo*
        root==0a0.dev0 depends on bar[some-extra]*
          foo==0.1.0 depends on anyio==4.1.0
            bar[some-extra]==0.1.0 depends on anyio==4.2.0
            no versions of bar[some-extra]<0.1.0 | >0.1.0

    ----- stderr -----
    Using Python 3.12.[X] interpreter at: [PYTHON-3.12]
      × No solution found when resolving dependencies:
      ╰─▶ Because only bar[some-extra]==0.1.0 is available and bar[some-extra] depends on anyio==4.2.0, we can conclude that all versions of bar[some-extra] depend on anyio==4.2.0.
          And because foo depends on anyio==4.1.0, we can conclude that foo and all versions of bar[some-extra] are incompatible.
          And because your workspace requires bar[some-extra] and foo, we can conclude that your workspace's requirements are unsatisfiable.
    "###
    );
```

---

_Label `internal` added by @zanieb on 2024-08-15 17:38_

---

_Label `tracing` added by @zanieb on 2024-08-15 17:38_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-15 17:55_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/error.rs`:256 on 2024-08-15 19:23_

You can also use `anstream::println`.

---

_@charliermarsh reviewed on 2024-08-15 19:23_

---

_@charliermarsh reviewed on 2024-08-15 19:24_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/error.rs`:261 on 2024-08-15 19:24_

Maybe to stderr?

---

_@charliermarsh approved on 2024-08-15 19:24_

---

_@zanieb reviewed on 2024-08-15 22:27_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:261 on 2024-08-15 22:27_

It's nice that it's separate from the rest of the command output right now, but yeah that's a logical place for it. Hm.

---

_Merged by @zanieb on 2024-08-16 14:25_

---

_Closed by @zanieb on 2024-08-16 14:25_

---

_Branch deleted on 2024-08-16 14:25_

---
