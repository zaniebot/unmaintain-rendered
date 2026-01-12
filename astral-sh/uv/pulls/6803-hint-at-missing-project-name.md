```yaml
number: 6803
title: "Hint at missing `project.name`"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/hint-for-common-toml-error
created_at: 2024-08-29T10:54:26Z
updated_at: 2024-09-14T20:20:39Z
url: https://github.com/astral-sh/uv/pull/6803
synced_at: 2026-01-12T16:07:32Z
```

# Hint at missing `project.name`

---

_@konstin_

We got user reports where users were confused about why they can't use `[project.urls]` in `pyproject.toml` (i think that's from poetry?). This PR adds a hint that (according to PEP 621), you need to set `project.name` when using any `project` fields. (PEP 621 also requires `project.version` xor `dynamic = ["version"]`, but we check that later.)

The intermediate parsing layer to tell apart syntax errors from schema errors doesn't incur a performance penalty according to epage (https://github.com/toml-rs/toml/issues/778#issuecomment-2310369253).

Closes #6419
Closes #6760

---

_@konstin reviewed on 2024-08-29 10:56_

---

_Review comment by @konstin on `crates/uv-build/src/lib.rs`:86 on 2024-08-29 10:56_

We only have `name`/`version`/`dynamic` here with the last two being optional, so we skip the check that's it's actually `name` here (in the other case we have to check for name to not confuse it with e.g. invalid `project.dependencies`; it's only a heuristic but imho good enough for the error hinting path)

---

_Label `error messages` added by @zanieb on 2024-08-29 13:42_

---

_Review requested from @zanieb by @zanieb on 2024-08-29 13:42_

---

_Comment by @jooon on 2024-08-29 22:54_

This hint seems to only be shown when a project is explicitly built.

```
$ cat pyproject.toml
[project.urls]
repository = 'https://github.com/octocat/octocat-python'
$ cargo run -- pip install .
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.20s
     Running `/home/jon/src/github.com/astral-sh/uv/target/debug/uv pip install .`
error: Failed to extract static metadata from `pyproject.toml`
  Caused by: `pyproject.toml` is using the `[project]` table, but the required `project.name` is not set.
  Caused by: TOML parse error at line 1, column 2
  |
1 | [project.urls]
  |  ^^^^^^^
missing field `name`
```

Not, when it is just parsed because of uv run.

```
$ cargo run -- run python
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.20s
     Running `/home/jon/src/github.com/astral-sh/uv/target/debug/uv run python`
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 1, column 2
  |
1 | [project.urls]
  |  ^^^^^^^
missing field `name`
```

---

_Comment by @konstin on 2024-08-30 07:06_

This is modelled after #6419 and #6760, where the failing cases were dependencies, not the project itself

---

_Comment by @charliermarsh on 2024-08-30 13:03_

@konstin -- Do you think we can also add this for the workspace discovery errors?

---

_@charliermarsh approved on 2024-09-14 19:54_

---

_Merged by @charliermarsh on 2024-09-14 20:03_

---

_Closed by @charliermarsh on 2024-09-14 20:03_

---

_Branch deleted on 2024-09-14 20:03_

---

_Comment by @charliermarsh on 2024-09-14 20:20_

I extended it to `uv run` in https://github.com/astral-sh/uv/pull/7399.

---
