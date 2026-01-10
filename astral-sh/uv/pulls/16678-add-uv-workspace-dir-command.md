```yaml
number: 16678
title: "Add `uv workspace dir` command"
type: pull_request
state: merged
author: mikaylathompson
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: mikayla/workspace-dir
created_at: 2025-11-11T00:30:12Z
updated_at: 2025-11-11T19:31:14Z
url: https://github.com/astral-sh/uv/pull/16678
synced_at: 2026-01-10T06:28:12Z
```

# Add `uv workspace dir` command

---

_Pull request opened by @mikaylathompson on 2025-11-11 00:30_

Addresses https://github.com/astral-sh/uv/issues/13636

Prints the path to the workspace root by default, and any of the child packages if requested.

It has a preview flag like `workspace metadata`, called `workspace-dir`.

## Summary

```
─> uv workspace dir
/Users/mikayla/code/uv/dev-envs

─> uv workspace dir --package foo-proj
/Users/mikayla/code/uv/dev-envs/foo-proj

─> uv workspace dir --package bar-proj
error: Package `bar-proj` not found in workspace.
```

## Test Plan

Unit tests added.


---

_Marked ready for review by @mikaylathompson on 2025-11-11 16:39_

---

_@zanieb reviewed on 2025-11-11 16:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/workspace/dir.rs`:37 on 2025-11-11 16:50_

I'm not sure if we should use portable paths here — that'll display a Unix-style path on Windows which matters for something like a TOML file or JSON output but may be weird in this context.

---

_@zanieb reviewed on 2025-11-11 16:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/workspace/dir.rs`:22 on 2025-11-11 16:50_

I think this should be a separate preview flag so we can stabilize things separately

---

_@zanieb reviewed on 2025-11-11 16:51_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1742 on 2025-11-11 16:51_

I think it's standard to unpack the args here, I don't think we want to pass the CLI type into implementations (just to be consistent)

---

_Review comment by @zanieb on `crates/uv/tests/it/workspace_dir.rs`:11 on 2025-11-11 16:54_

We usually read the manifest directory

https://github.com/astral-sh/uv/blob/8b479efd2fd03d5f92ce68e9cd98cf27a23ebc82/crates/uv/tests/it/common/mod.rs#L621-L628

I'd move that part out into a helper in `it/common/mod.rs`

---

_@zanieb reviewed on 2025-11-11 16:54_

---

_@zanieb reviewed on 2025-11-11 16:55_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6873 on 2025-11-11 16:55_

I think this needs a comment for the help menu

---

_@zanieb reviewed on 2025-11-11 16:57_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6856 on 2025-11-11 16:57_

I'd say a bit more 

```suggestion
    /// Display the path of a workspace member.
    ///
    /// By default, the path to the workspace root directory is displayed.
    /// The `--package` option can be used to display the path to a workspace member instead.
    ///
    /// If used outside of a workspace, i.e., if a `pyproject.toml` cannot be found, uv will exit with an error.
```

---

_@zanieb reviewed on 2025-11-11 16:57_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:6853 on 2025-11-11 16:57_

(My own doc here is bad, I should fix that)

---

_@zanieb reviewed on 2025-11-11 16:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/workspace/dir.rs`:44 on 2025-11-11 16:57_

I wonder if we should use color here? I think we usually use a color for paths.

---

_@zanieb approved on 2025-11-11 16:57_

---

_Label `cli` added by @zanieb on 2025-11-11 16:59_

---

_Label `preview` added by @zanieb on 2025-11-11 16:59_

---

_@mikaylathompson reviewed on 2025-11-11 17:02_

---

_Review comment by @mikaylathompson on `crates/uv/src/commands/workspace/dir.rs`:44 on 2025-11-11 17:02_

Oh, good to know, I'll dig around some other commands and try to match that

---

_@mikaylathompson reviewed on 2025-11-11 17:19_

---

_Review comment by @mikaylathompson on `crates/uv/src/commands/workspace/dir.rs`:37 on 2025-11-11 17:19_

Ahh, okay, I had not thought about why the JSON behavior would be different, but this makes sense. I'm pulling in more of the approach from `python dir` and `tool dir` to update this.

---

_Comment by @codspeed-hq[bot] on 2025-11-11 17:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/mikayla%2Fworkspace-dir?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16678 will **not alter performance**

<sub>Comparing <code>mikayla/workspace-dir</code> (e249d84) with <code>main</code> (63ab247)</sub>



### Summary

`✅ 6` untouched  





---

_Review comment by @konstin on `crates/uv/src/commands/workspace/dir.rs`:43 on 2025-11-11 18:34_

This code compiles without the type cast:

```suggestion
    let dir = match package_name {
        None => workspace.install_path(),
        Some(package) => {
            if let Some(p) = workspace.packages().get(&package) {
                p.root()
            } else {
                bail!("Package `{package}` not found in workspace.")
            }
        }
    };
```

---

_Review comment by @konstin on `crates/uv/tests/it/workspace_dir.rs`:30 on 2025-11-11 18:35_

nit:

```suggestion
/// Workspace dir output when run with `--package`.
```

---

_@konstin reviewed on 2025-11-11 18:36_

---

_Merged by @mikaylathompson on 2025-11-11 19:30_

---

_Closed by @mikaylathompson on 2025-11-11 19:30_

---

_Branch deleted on 2025-11-11 19:30_

---

_Renamed from "`workspace dir` command" to "Add `uv workspace dir` command" by @zanieb on 2025-11-11 19:31_

---
