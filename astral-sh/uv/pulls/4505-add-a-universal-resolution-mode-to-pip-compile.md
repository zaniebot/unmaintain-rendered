```yaml
number: 4505
title: "Add a universal resolution mode to `pip compile`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/universal
created_at: 2024-06-25T01:56:20Z
updated_at: 2024-06-25T21:28:51Z
url: https://github.com/astral-sh/uv/pull/4505
synced_at: 2026-01-12T16:06:16Z
```

# Add a universal resolution mode to `pip compile`

---

_@charliermarsh_

## Summary

This needs more tests and a few more changes, but checkpointing it for now.


---

_Renamed from "Add a universal resolution mode to pip compile" to "Add a universal resolution mode to `pip compile`" by @charliermarsh on 2024-06-25 01:56_

---

_Label `enhancement` added by @charliermarsh on 2024-06-25 01:56_

---

_@charliermarsh reviewed on 2024-06-25 01:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:6350 on 2024-06-25 01:56_

Need to get the Python version filtering working like we have for `uv lock`.

---

_Marked ready for review by @charliermarsh on 2024-06-25 12:00_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:575 on 2024-06-25 12:03_

:tada: 

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:572 on 2024-06-25 12:04_

"Python platforms" is likely unclear to the users, we likely need something along the lines of "all operating systems, architectures and supported Python versions, as supported by your dependencies"

---

_@konstin approved on 2024-06-25 12:06_

Hyped for supporting this

---

_@charliermarsh reviewed on 2024-06-25 12:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/python_requirement.rs`:82 on 2024-06-25 12:19_

I'm a bit unsure of this. It still locks against the target Python version rather than "the target version at minimum." It's easy to change but not sure what the right semantics are.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-25 12:20_

---

_@konstin reviewed on 2024-06-25 12:36_

---

_Review comment by @konstin on `crates/uv-resolver/src/python_requirement.rs`:82 on 2024-06-25 12:36_

My feeling is:
* `requires-python` if the input is a `pyproject.toml` (the conjunction of all `requires-python` values if there are multiple `pyproject.toml`).
* An explicit `--requires-python` argument.
* `>={{current python}}` and inform the user that the should consider passing `--requires-python` if they want an interpreter independent resolution.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:200 on 2024-06-25 13:50_

Nice. You got rid of the `MarkerTree::And(vec![])`! I think the change is for the better right? My understanding is that now it will always have some non-empty marker expression in universal mode. So that when `Requires-Python` isn't provided, it will be based on the installed version of Python? Where as previously, there would be no filtering at all.

---

_@BurntSushi approved on 2024-06-25 13:57_

![that-was-easy-pressing-easy-button-5v06l34nfgfg8q2f](https://github.com/astral-sh/uv/assets/456674/f5344c57-c6e8-4b6b-af53-769c69f725b9)


---

_Merged by @charliermarsh on 2024-06-25 21:28_

---

_Closed by @charliermarsh on 2024-06-25 21:28_

---

_Branch deleted on 2024-06-25 21:28_

---
