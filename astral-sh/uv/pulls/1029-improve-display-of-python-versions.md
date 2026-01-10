```yaml
number: 1029
title: Improve display of Python versions
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/target-version
created_at: 2024-01-21T17:39:13Z
updated_at: 2024-01-22T18:46:19Z
url: https://github.com/astral-sh/uv/pull/1029
synced_at: 2026-01-10T15:39:03Z
```

# Improve display of Python versions

---

_Pull request opened by @zanieb on 2024-01-21 17:39_

In https://github.com/astral-sh/puffin/pull/986 there was some confusion about what these values are set to and I noticed that we never actually display the target version being used for a resolution.

- Consistently display the Python interpreter being used, i.e. make it clear that we are referring the the interpreter/installed Python version and always show the version number
- Display the target Python version during solving


---

_@charliermarsh approved on 2024-01-21 18:08_

Looks great.

---

_@charliermarsh reviewed on 2024-01-21 18:09_

---

_Review comment by @charliermarsh on `crates/puffin/src/commands/freeze.rs`:24 on 2024-01-21 18:09_

I would vote to remove `interpreter`

---

_Review comment by @charliermarsh on `crates/puffin/src/commands/freeze.rs`:26 on 2024-01-21 18:09_

Can you omit `.display()` and just use `.cyan()`? (I can't remember.)

---

_@charliermarsh reviewed on 2024-01-21 18:09_

---

_@zanieb reviewed on 2024-01-21 23:15_

---

_Review comment by @zanieb on `crates/puffin/src/commands/freeze.rs`:24 on 2024-01-21 23:15_

It's confusing during resolution otherwise, since we say "Using Python ..." which makes it unclear what we're using that version of Python for. It doesn't quite matter here, but it seemed better to say "interpreter" everywhere? I'm not sure.

---

_@zanieb reviewed on 2024-01-22 16:45_

---

_Review comment by @zanieb on `crates/puffin/src/commands/freeze.rs`:26 on 2024-01-22 16:45_

Nope.

```
error[E0277]: `PathBuf` doesn't implement `std::fmt::Display`
  --> crates/puffin/src/commands/freeze.rs:26:9
   |
26 |         venv.python_executable().cyan()
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ `PathBuf` cannot be formatted with the default formatter; call `.display()` on it
   |
```

---

_Merged by @zanieb on 2024-01-22 18:46_

---

_Closed by @zanieb on 2024-01-22 18:46_

---

_Branch deleted on 2024-01-22 18:46_

---
