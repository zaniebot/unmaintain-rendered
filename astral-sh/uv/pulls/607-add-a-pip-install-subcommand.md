```yaml
number: 607
title: "Add a `pip-install` subcommand"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pip-install
created_at: 2023-12-11T19:23:07Z
updated_at: 2023-12-12T17:16:02Z
url: https://github.com/astral-sh/uv/pull/607
synced_at: 2026-01-10T15:44:44Z
```

# Add a `pip-install` subcommand

---

_Pull request opened by @charliermarsh on 2023-12-11 19:23_

## Summary

This PR adds a `pip-install` command that operates like, well, `pip install`. In short, it resolves the provided dependency, then makes sure they're all installed in the environment. The primary differences with `pip-sync` are that (1) `pip-sync` ignores dependencies, and assumes that the packages represent a complete set; and (2) `pip-sync` uninstalls any unlisted packages.

There are a bunch of TODOs that I'll resolve in subsequent PRs.

Closes https://github.com/astral-sh/puffin/issues/129.


---

_Review requested from @zanieb by @charliermarsh on 2023-12-11 19:23_

---

_Review requested from @konstin by @charliermarsh on 2023-12-11 19:23_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/mod.rs`:76 on 2023-12-11 19:23_

(Moved out of `pip_sync.rs`.)

---

_@charliermarsh reviewed on 2023-12-11 19:23_

---

_@charliermarsh reviewed on 2023-12-11 19:24_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_install.rs`:431 on 2023-12-11 19:24_

There's some code that's repeated between this file and `pip_sync.rs` and `pip_compile.rs`, but honestly, I would rather repeat it here than try to DRY it up. The code is honestly fairly straightforward and mechanical, and I'd hate to introduce a bunch of abstractions to DRY it up while capturing the differences.

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/mod.rs`:67 on 2023-12-12 04:29_

nit: I know this isn't new here, but events feel like they shouldn't be imperative e.g. use `Removed`, `Added` instead.

---

_Review comment by @zanieb on `crates/puffin-cli/src/main.rs`:278 on 2023-12-12 04:32_

Perhaps note what will happen if wheels are not available?

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install.rs`:108 on 2023-12-12 04:35_

Unnecessary, right?
```suggestion
    requirements_txt.write_str("Flask")?;
```
 e.g. https://github.com/astral-sh/puffin/blob/2d1e19e474cbbbcd0008e150988807f893aacada/crates/puffin-cli/tests/pip_compile.rs#L163-L164

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install.rs`:146 on 2023-12-12 04:37_

Why use the requirements txt syntax to install single requirements? Doesn't this support specifying packages to install directly?

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install.rs`:1 on 2023-12-12 04:39_

It looks like we're missing a lot of test coverage here

---

_@zanieb approved on 2023-12-12 04:43_

---

_@charliermarsh reviewed on 2023-12-12 04:47_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_install.rs`:1 on 2023-12-12 04:47_

I don't think it's worth duplicating all of the resolution and installation tests here, personally.

---

_@zanieb reviewed on 2023-12-12 04:50_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install.rs`:1 on 2023-12-12 04:50_

I agree, but like... `pip-install foo` seems reasonable to include at least.

---

_Review comment by @konstin on `crates/puffin-cli/src/main.rs`:301 on 2023-12-12 10:10_

We could allow this module-wide

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_install.rs`:98 on 2023-12-12 10:14_

This is ok for now, but we should eventually move to passing more information, e.g. files attached to the requirements

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_install.rs`:431 on 2023-12-12 10:22_

I'd refactor reporter handling (move it to the constructor most likely) if we want to make this more concise.

---

_@konstin approved on 2023-12-12 10:23_

---

_Merged by @charliermarsh on 2023-12-12 17:16_

---

_Closed by @charliermarsh on 2023-12-12 17:16_

---

_Branch deleted on 2023-12-12 17:16_

---
