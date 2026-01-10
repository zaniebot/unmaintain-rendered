```yaml
number: 4430
title: "Flatten requirements eagerly in `get_dependencies`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/fork-before-transforming
created_at: 2024-06-20T18:34:46Z
updated_at: 2024-06-25T21:13:48Z
url: https://github.com/astral-sh/uv/pull/4430
synced_at: 2026-01-10T13:48:28Z
```

# Flatten requirements eagerly in `get_dependencies`

---

_Pull request opened by @konstin on 2024-06-20 18:34_

Downstack PR: #4515 Upstack PR: #4481

Consider these two cases:

A:
```
werkzeug==2.0.0
werkzeug @ https://files.pythonhosted.org/packages/ff/1d/960bb4017c68674a1cb099534840f18d3def3ce44aed12b5ed8b78e0153e/Werkzeug-2.0.0-py3-none-any.whl
```

B:
```toml
dependencies = [
  "iniconfig == 1.1.1 ; python_version < '3.12'",
  "iniconfig @ git+https://github.com/pytest-dev/iniconfig@93f5930e668c0d1ddf4597e38dd0dea4e2665e7a ; python_version >= '3.12'",
]
```

In the first case, `werkzeug==2.0.0` should be overridden by the url. In the second case `iniconfig == 1.1.1` is in a different fork and must remain a registry distribution.

That means the conversion from `Requirement` to `PubGrubPackage` is dependent on the other requirements of the package. We can either look into the other packages immediately, or we can move the forking before the conversion to `PubGrubDependencies` instead of after. Either version requires a flat list of `Requirement`s to use. This refactoring gives us this list.

I'll add support for both of the above cases in the forking urls branch before merging this PR. I also have to move constraints over to this.


---

_Review requested from @BurntSushi by @konstin on 2024-06-20 18:34_

---

_@BurntSushi approved on 2024-06-20 18:45_

This makes sense to me.

---

_Comment by @zanieb on 2024-06-20 18:51_

Does this affect non-forking resolution?

---

_Comment by @konstin on 2024-06-20 19:17_

> Does this affect non-forking resolution?

If i implemented everything correctly, no, then it's just a refactoring,

---

_@charliermarsh approved on 2024-06-23 17:49_

---

_@charliermarsh reviewed on 2024-06-23 18:05_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1247 on 2024-06-23 18:05_

Looks like an extra debug statement here.

---

_@charliermarsh reviewed on 2024-06-23 21:50_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:974 on 2024-06-23 21:50_

Where did all of this go?

---

_@charliermarsh reviewed on 2024-06-23 21:50_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1094 on 2024-06-23 21:50_

What does this mean? Can I help explain it?

---

_@charliermarsh reviewed on 2024-06-23 21:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:974 on 2024-06-23 21:51_

Oh, it was moved to the bottom. Could that be a separate PR?

---

_@konstin reviewed on 2024-06-25 18:57_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1094 on 2024-06-25 18:57_

Yes please, my question is: For the extras, we add the actual requirements from the extra to the dependencies; for dev-dependencies, why do we only add the proxy package?

---

_Merged by @konstin on 2024-06-25 21:13_

---

_Closed by @konstin on 2024-06-25 21:13_

---

_Branch deleted on 2024-06-25 21:13_

---
