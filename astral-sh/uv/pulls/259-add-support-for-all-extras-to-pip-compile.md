```yaml
number: 259
title: "Add support for `--all-extras` to `pip-compile`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/all-extra
created_at: 2023-10-31T19:34:28Z
updated_at: 2023-11-01T18:39:50Z
url: https://github.com/astral-sh/uv/pull/259
synced_at: 2026-01-12T16:03:50Z
```

# Add support for `--all-extras` to `pip-compile`

---

_@zanieb_

Closes #244

Notable decision to error if `--all-extra` and `--extra <name>` are both provided.

---

_@zanieb reviewed on 2023-10-31 20:27_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:106 on 2023-10-31 20:27_

Simple optimization to avoid iterating over (and parsing) the optional dependency groups if the user doesn't want any of them; the rest of this section is indented without change.

---

_@charliermarsh reviewed on 2023-10-31 21:01_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/main.rs`:77 on 2023-10-31 21:01_

Does Clap allow us to also add `conflicts_with = "extra"` on `all_extras`? I know it's redundant but I find the symmetry a bit clearer.

---

_@charliermarsh reviewed on 2023-10-31 21:01_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/main.rs`:215 on 2023-10-31 21:01_

Nit: maybe collapse this to an `else if`?

---

_@charliermarsh approved on 2023-10-31 21:01_

Nice, looks great.

---

_@zanieb reviewed on 2023-11-01 00:47_

---

_Review comment by @zanieb on `crates/puffin-cli/src/main.rs`:77 on 2023-11-01 00:47_

Yeah I think we usually do that in Ruff so I can give it a try — don't mind as long as the error message is nice.

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_compile.rs`:57 on 2023-11-01 12:03_

What about using a `BTreeSet` here? Not for performance but because it's more concise

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:467 on 2023-11-01 12:04_

I like to [indoc](https://docs.rs/indoc/latest/indoc/) for those, but it's a matter of taste so entirely your choice

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:483 on 2023-11-01 12:05_

Since we use the same replacements everywhere, do we want to assign to a const or static? I check and it doesn't match on the expression contents: https://docs.rs/insta/latest/src/insta/macros.rs.html#515

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_compile__compile_does_not_allow_both_extra_and_all_extras.snap`:25 on 2023-11-01 12:13_

Could we inline this snapshot? I should be short enough without the info block on top and for error message test i find it helpful to have the output next to the test

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_compile__compile_pyproject_toml_all_extras.snap`:10 on 2023-11-01 12:14_

temp path

---

_@konstin approved on 2023-11-01 12:14_

---

_@charliermarsh reviewed on 2023-11-01 12:39_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/snapshots/pip_compile__compile_pyproject_toml_all_extras.snap`:10 on 2023-11-01 12:39_

These aren’t actually used for validation, since they’re just part of the input command. I think removing them is nice but not fully required.

---

_@charliermarsh reviewed on 2023-11-01 13:26_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:57 on 2023-11-01 13:26_

I suggested using a `Vec` here originally, I preferred it over the set-based approach personally.

---

_@konstin reviewed on 2023-11-01 13:44_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:483 on 2023-11-01 13:44_

Done in #270

---

_@zanieb reviewed on 2023-11-01 15:22_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:483 on 2023-11-01 15:22_

Yeah these test cases feel way heavier than they need to be. Thanks!

---

_@zanieb reviewed on 2023-11-01 15:22_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:467 on 2023-11-01 15:22_

Oh interesting, I didn't know about that. Will consider that separately; here (as before) I'm just copying and pasting the existing test pattern.

---

_@zanieb reviewed on 2023-11-01 15:23_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_does_not_allow_both_extra_and_all_extras.snap`:25 on 2023-11-01 15:23_

I agree inline snapshots seem preferable often.. let me see.

---

_@zanieb reviewed on 2023-11-01 15:40_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_pyproject_toml_all_extras.snap`:10 on 2023-11-01 15:40_

Yeah this is the same in all of our existing snapshots

---

_@konstin reviewed on 2023-11-01 16:38_

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_compile__compile_pyproject_toml_all_extras.snap`:10 on 2023-11-01 16:38_

What would you think about just removing the header from all the snapshots? (In another PR, this one is fine)

---

_Closed by @konstin on 2023-11-01 16:39_

---

_Reopened by @konstin on 2023-11-01 16:39_

---

_@zanieb reviewed on 2023-11-01 18:39_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_pyproject_toml_all_extras.snap`:10 on 2023-11-01 18:39_

No preference from me

---

_Merged by @zanieb on 2023-11-01 18:39_

---

_Closed by @zanieb on 2023-11-01 18:39_

---

_Branch deleted on 2023-11-01 18:39_

---
