```yaml
number: 3864
title: "Add `uv run --package`"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/workspaces-run-in-different-package
created_at: 2024-05-27T13:29:50Z
updated_at: 2024-06-02T21:42:16Z
url: https://github.com/astral-sh/uv/pull/3864
synced_at: 2026-01-12T16:05:54Z
```

# Add `uv run --package`

---

_@konstin_

Add a `--package` option that allows switching the current project in the workspace. Wherever you are in a workspace, you should be able to run with any other project as root. This is the uv equivalent of `cargo run -p`.

I don't love the `--package` name, esp. since `-p` is already taken and in general to many things start with p already.

Part of this change is moving the workspace discovery of `ProjectWorkspace` to `Workspace` itself.

## Usage

In albatross-virtual-workspace:

```console
$ uv venv
$ uv run --preview --package bird-feeder python -c "import albatross"
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/bird-feeder
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/seeds
Built 2 editables in 167ms
Resolved 5 packages in 4ms
Installed 5 packages in 1ms
 + anyio==4.4.0
 + bird-feeder==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/bird-feeder)
 + idna==3.6
 + seeds==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/seeds)
 + sniffio==1.3.1
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'albatross'
$ uv venv
$ uv run --preview --package albatross python -c "import albatross"
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/albatross
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/bird-feeder
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/seeds
Built 3 editables in 173ms
Resolved 7 packages in 6ms
Installed 7 packages in 1ms
 + albatross==0.1.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/albatross)
 + anyio==4.4.0
 + bird-feeder==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/bird-feeder)
 + idna==3.6
 + seeds==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-virtual-workspace/packages/seeds)
 + sniffio==1.3.1
 + tqdm==4.66.4
```

In albatross-root-workspace:

```console
$ uv venv
$ uv run --preview --package bird-feeder python -c "import albatross"
  Using Python 3.12.3 interpreter at: /home/konsti/.local/bin/python3
  Creating virtualenv at: .venv
  Activate with: source .venv/bin/activate
      Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
       Running `/home/konsti/projects/uv/target/debug/uv run --preview --package bird-feeder python -c 'import albatross'`
     Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/bird-feeder
     Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/seeds                                              Built 2 editables in 161ms
  Resolved 5 packages in 4ms
  Installed 5 packages in 1ms
   + anyio==4.4.0
   + bird-feeder==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/bird-feeder)
   + idna==3.6
   + seeds==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/seeds)
   + sniffio==1.3.1
  Traceback (most recent call last):
    File "<string>", line 1, in <module>
  ModuleNotFoundError: No module named 'albatross'
$ uv venv
$ cargo run run --preview --package albatross python -c "import albatross"
Using Python 3.12.3 interpreter at: /home/konsti/.local/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `/home/konsti/projects/uv/target/debug/uv run --preview --package albatross python -c 'import albatross'`
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/bird-feeder
   Built file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/seeds
Built 3 editables in 168ms
Resolved 7 packages in 5ms
Installed 7 packages in 1ms
 + albatross==0.1.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace)
 + anyio==4.4.0
 + bird-feeder==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/bird-feeder)
 + idna==3.6
 + seeds==1.0.0 (from file:///home/konsti/projects/uv/scripts/workspaces/albatross-root-workspace/packages/seeds)
 + sniffio==1.3.1
 + tqdm==4.66.4
```


---

_Label `preview` added by @konstin on 2024-05-27 13:29_

---

_Comment by @codspeed-hq[bot] on 2024-05-27 14:34_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/workspaces-run-in-different-package)

### Merging #3864 will **improve performances by 6.72%**

<sub>Comparing <code>konsti/workspaces-run-in-different-package</code> (742440e) with <code>main</code> (0d0308c)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/workspaces-run-in-different-package` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 926.7 ns | 868.3 ns | +6.72% |


---

_Marked ready for review by @konstin on 2024-05-28 09:11_

---

_@konstin reviewed on 2024-05-28 09:12_

---

_Review comment by @konstin on `crates/uv/tests/workspace.rs`:40 on 2024-05-28 09:12_

This is a major drawback from merging this, the snapshots aren't stable yet

---

_Review requested from @charliermarsh by @konstin on 2024-05-28 16:32_

---

_@charliermarsh approved on 2024-06-01 20:12_

---

_Comment by @charliermarsh on 2024-06-01 20:27_

Not sure what's up with the Windows failure.

---

_Comment by @charliermarsh on 2024-06-01 20:38_

It passes without error on my Windows machine.

---

_Comment by @charliermarsh on 2024-06-01 23:39_

Some bad bug here because Windows is getting the following members (the names don't match the contents):

```
Found members: `{PackageName("albatross"): WorkspaceMember { root: "C:\/Users\/RUNNER~1\/AppData\/Local\/Temp\/[TMP]/seeds", pyproject_toml: PyProjectToml { project: Some(Project { name: PackageName("seeds"), optional_dependencies: None }), tool: None } }}`
```

---

_Comment by @charliermarsh on 2024-06-02 20:38_

Sorry, I think I was being misled by the filters. I suspect the bug is in comparing the installed vs. requested path or something.

---

_Comment by @charliermarsh on 2024-06-02 20:42_

Like we're using absolute vs. canonical paths either when defining the requirement or installing, but not being consistent between them.

---

_Comment by @charliermarsh on 2024-06-02 20:56_

Okay yeah, the installed distribution has: `/C:/Users/RUNNER~1/AppData/Local/Temp/.tmpJTJbWC/albatross-root-workspace/packages/bird-feeder`

But the requirement is: `C:\/Users\/runneradmin\/AppData\/Local\/Temp\\.tmpJTJbWC\/albatross-root-workspace\/packages\/bird-feeder`

---

_Comment by @charliermarsh on 2024-06-02 21:21_

I hopefully fixed it by respecting symlinks in `RequirementSatisfaction::check`. I should look at what pip does with symlinks though, whether it records them under the canonical path, etc.

---

_Merged by @charliermarsh on 2024-06-02 21:42_

---

_Closed by @charliermarsh on 2024-06-02 21:42_

---

_Branch deleted on 2024-06-02 21:42_

---
