```yaml
number: 5197
title: "Add `uv tool upgrade` command"
type: pull_request
state: merged
author: blueraft
labels: []
assignees: []
merged: true
base: main
head: upgrade-tool
created_at: 2024-07-18T18:58:04Z
updated_at: 2024-08-08T20:48:15Z
url: https://github.com/astral-sh/uv/pull/5197
synced_at: 2026-01-10T13:31:54Z
```

# Add `uv tool upgrade` command

---

_Pull request opened by @blueraft on 2024-07-18 18:58_

## Summary

Resolves #5188. Most of the changes involve creating a new function in `tool/common.rs` to contain the common functionality previously found in `tool/install.rs`.

## Test Plan

`cargo test`

```console
❯ ./target/debug/uv tool upgrade black
warning: `uv tool upgrade` is experimental and may change without warning.
Resolved 6 packages in 25ms
Uninstalled 1 package in 3ms
Installed 1 package in 19ms
 - black==23.1.0
 + black==24.4.2
Installed 2 executables: black, blackd
```


---

_@zanieb reviewed on 2024-07-18 19:00_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2729 on 2024-07-18 19:00_

Does including this mean we support things like `--upgrade-package` here now? Is that what we want?

---

_@zanieb reviewed on 2024-07-18 19:01_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:305 on 2024-07-18 19:01_

```suggestion
/// The resolved settings to use for a `tool upgrade` invocation.
```

---

_@zanieb reviewed on 2024-07-18 19:02_

---

_Review comment by @zanieb on `crates/uv/tests/tool_upgrade.rs`:59 on 2024-07-18 19:02_

Should these say "Updated"?

---

_@zanieb reviewed on 2024-07-18 19:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/upgrade.rs`:22 on 2024-07-18 19:04_

I'll need to come back and read this more closely, sorry for a partial review.

---

_@blueraft reviewed on 2024-07-18 20:15_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:2729 on 2024-07-18 20:15_

Good point. The following approach works, and there are cases where people might want to upgrade tools to a specific version as well. Maybe we should support this, wdyt?

```cargo run -- tool upgrade poetry --upgrade-package poetry==1.5```

---

_@T-256 reviewed on 2024-07-18 21:58_

---

_Review comment by @T-256 on `crates/uv/tests/tool_upgrade.rs`:59 on 2024-07-18 21:58_

```
Installed [N] packages in [TIME]
 - black==23.1.0
 + black==24.3.0
Installed 3 executables:
 ^ black
 ^ blackd
 + added-exe
 - removed-exe
```
same message format as package (un)installations diffs.
`^` mark stands for _Updated_ executable.

---

_@T-256 reviewed on 2024-07-18 22:02_

---

_Review comment by @T-256 on `crates/uv/tests/tool_upgrade.rs`:59 on 2024-07-18 22:02_


> `^` mark stands for _Updated_ executable.


As I'm thinking now, could we have this _Updated_ versions in package installations? example:
```
Installed [N] packages in [TIME]
 ^ black 23.1.0 -> 24.3.0
```


---

_@blueraft reviewed on 2024-07-19 09:26_

---

_Review comment by @blueraft on `crates/uv/tests/tool_upgrade.rs`:59 on 2024-07-19 09:26_

> As I'm thinking now, could we have this Updated versions in package installations? example:

That requires changes to the resolver code, could you make a new issue if you want this change?

---

_@blueraft reviewed on 2024-07-19 13:05_

---

_Review comment by @blueraft on `crates/uv/tests/tool_upgrade.rs`:59 on 2024-07-19 13:05_

I've gone with `Updated` for now. Once a symbol is settled in #5213 for `updated`, we can update this. Wdyt?

---

_Assigned to @zanieb by @zanieb on 2024-07-23 18:10_

---

_Comment by @charliermarsh on 2024-07-24 16:15_

Should `uv tool upgrade` work without arguments? That might mean that we need to store things like the index URLs in the install receipt. Otherwise, upgrades for tools installed from non-PyPI indexes won't work, unless you provide the index URL to `uv tool upgrade`.

---

_Comment by @zanieb on 2024-07-24 19:44_

Yeah we probably need to store index URLs in the receipt 🤔 does pipx?

---

_Comment by @blueraft on 2024-07-25 12:08_

These are the metadata stored by pipx, arguments like `--index-url` end up in the pip_args array:
```json
{
    "injected_packages": {},
    "main_package": {
        "app_paths": [
            {
                "__Path__": "/Users/ahmedilyas/Library/Application Support/pipx/venvs/sympy/bin/isympy",
                "__type__": "Path"
            }
        ],
        "app_paths_of_dependencies": {},
        "apps": [
            "isympy"
        ],
        "apps_of_dependencies": [],
        "include_apps": true,
        "include_dependencies": false,
        "man_pages": [
            "man1/isympy.1"
        ],
        "man_pages_of_dependencies": [],
        "man_paths": [
            {
                "__Path__": "/Users/ahmedilyas/Library/Application Support/pipx/venvs/sympy/share/man/man1/isympy.1",
                "__type__": "Path"
            }
        ],
        "man_paths_of_dependencies": {},
        "package": "sympy",
        "package_or_url": "sympy",
        "package_version": "1.13.1",
        "pip_args": [],
        "suffix": ""
    },
    "pipx_metadata_version": "0.3",
    "python_version": "Python 3.12.2",
    "venv_args": []
}
```

---

_Comment by @zanieb on 2024-08-07 22:03_

Just noting this is blocked by https://github.com/astral-sh/uv/issues/5443

---

_Comment by @charliermarsh on 2024-08-08 19:25_

I'm gonna be playing around with this branch as I'm trying to solve the "index URL" problem. Just a heads up @blueraft.

---

_Comment by @blueraft on 2024-08-08 19:36_

All good!

---

_@charliermarsh approved on 2024-08-08 20:25_

Thank you! Gonna build on this in some subsequent PRs. Merging with the known limitation that it doesn't respect prior settings.

---

_Merged by @charliermarsh on 2024-08-08 20:48_

---

_Closed by @charliermarsh on 2024-08-08 20:48_

---
