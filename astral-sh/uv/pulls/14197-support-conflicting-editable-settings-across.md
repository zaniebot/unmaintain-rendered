```yaml
number: 14197
title: Support conflicting editable settings across groups
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - breaking
assignees: []
merged: true
base: release/080
head: charlie/ed
created_at: 2025-06-22T02:15:54Z
updated_at: 2025-07-11T02:20:03Z
url: https://github.com/astral-sh/uv/pull/14197
synced_at: 2026-01-10T06:53:01Z
```

# Support conflicting editable settings across groups

---

_Pull request opened by @charliermarsh on 2025-06-22 02:15_

## Summary

If a user specifies `-e /path/to/dir` and `/path/to/dir` in a `uv pip install` command, we want the editable to "win" (rather than erroring due to conflicting URLs). Unfortunately, this behavior meant that when you requested a package as editable and non-editable in conflicting groups, the editable version was _always_ used. This PR modifies the requisite types to use `Option<bool>` rather than `bool` for the `editable` field, so we can determine whether a requirement was explicitly requested as editable, explicitly requested as non-editable, or not specified (as in the case of `/path/to/dir` in a `requirements.txt` file). In the latter case, we allow editables to override the "unspecified" requirement.

If a project includes a path dependency twice, once with `editable = true` and once without any `editable` annotation, those are now considered conflicting URLs, and lead to an error, so I've marked this change as breaking.

Closes https://github.com/astral-sh/uv/issues/14139.


---

_Label `bug` added by @charliermarsh on 2025-06-22 02:15_

---

_Review requested from @konstin by @charliermarsh on 2025-06-22 02:22_

---

_Comment by @konstin on 2025-06-22 13:06_

While the new behavior is a strict improvement, the new behavior still allows inactive groups to change the behavior of active groups. For example, the following two are different for `uv sync --group bar`, even through bar and foo are conflicting groups. I.e., adding an inactive conflicting group can changes defaults for an unchanged active conflicting group.

```toml
[tool.uv]
conflicts = [[{ group = "foo" }, { group = "bar" }]]

[tool.uv.sources]
child = [
  { path = "./child", editable = true, group = "foo" },
  { path = "./child", editable = false, group = "bar" },
]
```

```console
$ uv venv -q && uv-debug sync --group bar && uv-debug pip list
Resolved 3 packages in 2ms
Installed 2 packages in 4ms
 + child==0.1.0 (from file:///home/konsti/projects/uv/debug/child)
 + debug==0.1.0 (from file:///home/konsti/projects/uv/debug)
Package Version Editable project location
------- ------- ------------------------------
child   0.1.0
debug   0.1.0   /home/konsti/projects/uv/debug
```

```toml
[tool.uv]
conflicts = [[{ group = "foo" }, { group = "bar" }]]

[tool.uv.sources]
child = [
  { path = "./child", editable = true, group = "foo" },
  { path = "./child", group = "bar" },
]
```

```console
$ uv venv -q && uv-debug sync --group bar && uv-debug pip list
Resolved 2 packages in 7ms
      Built debug @ file:///home/konsti/projects/uv/debug
Prepared 1 package in 140ms
Installed 2 packages in 1ms
 + child==0.1.0 (from file:///home/konsti/projects/uv/debug/child)
 + debug==0.1.0 (from file:///home/konsti/projects/uv/debug)
Package Version Editable project location
------- ------- ------------------------------------
child   0.1.0   /home/konsti/projects/uv/debug/child
debug   0.1.0   /home/konsti/projects/uv/debug
```

This is different from other attributes in sources that are usually "sticky", such as URLs: A URL in one conflicting branch does not influence the other branch:

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12"

[dependency-groups]
foo = ["tqdm"]
bar = ["tqdm"]

[tool.uv]
conflicts = [[{ group = "foo" }, { group = "bar" }]]

[tool.uv.sources]
tqdm = [
  { url = "https://files.pythonhosted.org/packages/d0/30/dc54f88dd4a2b5dc8a0279bdd7270e735851848b762aeb1c1184ed1f6b14/tqdm-4.67.1-py3-none-any.whl", group = "foo" },
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Comment by @charliermarsh on 2025-06-22 14:08_

That seems okay though it’s also fixable — do you want me to change it?

---

_Comment by @charliermarsh on 2025-06-23 00:07_

@konstin -- Updated and added a test.

---

_Comment by @charliermarsh on 2025-06-23 17:14_

It looks like `lock_editable` now fails because we consider it a conflict to request once with `editable = true` and once without an `editable` annotation (since that gets treated as `editable = false`). That suggests to me that maybe we _should_ continue to allow `editable = true` to override the unset `editable`.

---

_Label `breaking` added by @charliermarsh on 2025-06-23 17:22_

---

_Comment by @charliermarsh on 2025-06-23 17:25_

Instead, I just marked this as breaking and changed the test. I think the behavior we have here is more understandable than doing anything with global / implicit overrides.

---

_@konstin reviewed on 2025-06-24 10:44_

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:11032 on 2025-06-24 10:44_

I'm confused how this used to pass but now shows two different paths but not a different editable?

---

_Comment by @konstin on 2025-06-24 10:45_

I'm really into the new behavior!

---

_@konstin reviewed on 2025-06-24 10:46_

Should we target this at the 0.8 branch?

We need some clarification on the new error message, otherwise r+

---

_@charliermarsh reviewed on 2025-06-28 13:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:11032 on 2025-06-28 13:23_

It used to pass because we let the editable override. Now there's a conflict because one dependency is requesting the editable and another is requesting a non-editable. Should we show `-e` for one of them, or something?

---

_Review requested from @konstin by @charliermarsh on 2025-06-28 13:23_

---

_Added to milestone `v0.8.0` by @charliermarsh on 2025-06-28 13:23_

---

_@konstin approved on 2025-06-30 14:03_

---

_Comment by @charliermarsh on 2025-06-30 14:22_

(I think I should still fix this to include the editable information in the error message)

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:11032 on 2025-06-30 14:28_

maybe `/path/to/file (editable)`? `-e` works too but might be hard to read with the existing leading `-`.

---

_@konstin reviewed on 2025-06-30 14:28_

---

_Merged by @charliermarsh on 2025-07-11 02:20_

---

_Closed by @charliermarsh on 2025-07-11 02:20_

---

_Branch deleted on 2025-07-11 02:20_

---
