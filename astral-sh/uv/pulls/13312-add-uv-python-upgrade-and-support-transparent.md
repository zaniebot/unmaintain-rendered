```yaml
number: 13312
title: "Add `uv python upgrade` and support transparent patch upgrades in virtual environments"
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/python-upgrade
created_at: 2025-05-06T10:58:56Z
updated_at: 2025-06-10T17:04:39Z
url: https://github.com/astral-sh/uv/pull/13312
synced_at: 2026-01-10T11:10:41Z
```

# Add `uv python upgrade` and support transparent patch upgrades in virtual environments

---

_Pull request opened by @jtfmumm on 2025-05-06 10:58_

In uv, if you install the latest managed patch version, virtual environments that were created with a minor version like `3.13` will not automatically upgrade to that patch. Currently, you’d have to manually update each virtual environment. Upgrading to a new patch version should be a smooth experience for users, ideally with virtual environments transparently upgrading.

This PR adds a new `uv python upgrade` subcommand. This can be used to upgrade an installed and managed `python-build-standalone` Python to the latest patch version:

```
uv python upgrade 3.12
```

If the minor version is omitted, uv will upgrade all (managed `python-build-standalone`) minor versions that are currently installed. 

Transparent upgrades also apply when a newer patch is installed via a command like `uv python install 3.10.17`.

Transparent upgrades are accomplished using minor version symlink directories (or junctions on Windows). On Windows, a trampoline is used to simulate the full symlink.

On Unix, imagine we have a Python 3.10.8 installation in `/path/to/uv/python/cpython-3.10.8-macos-aarch64-none/bin/python3.10`. In this case, the the symlink directory would be `/path/to/uv/python/python3.10` and the executable path including the symlink directory would be `/path/to/uv/python/python3.10/bin/python3.10`.

On Windows, imagine we have a Python 3.10.8 installation in `C:\path\to\uv\python\cpython-3.10.8-windows-x86_64-none\python.exe`. The junction would be `C:\path\to\uv\python\python3.10` and the executable path including the junction would be `C:\path\to\uv\python\python3.10\python.exe`.

Transparent upgrades are supported for virtual environments created
* with `uv venv`
* with `uv run python -m venv` (as long as the Python being run is managed and `python-build-standalone`)
* within virtual environments created in any of these ways

Virtual environments created by an earlier version of uv will continue to work as before but will not be transparently upgradeable without being recreated. 

A potential downside of the transparent upgrade approach is that breaking patch upgrades would break all transparently upgraded virtual environments. This is a rare situation, but it can be avoided altogether by pinning projects to a patch version. Upgrading does not remove the older patch versions, so pinning continues to work.

This PR includes tests for numerous scenarios using both `uv python upgrade` and `uv python install`. 

This PR does not currently support transparent upgrades for the `bin` directory symlink created by `uv python install --preview`. That support is added in #13531 (which also fixes the failing `uninstall_last_patch` on Windows).



---

_Label `enhancement` added by @jtfmumm on 2025-05-06 10:58_

---

_Label `breaking` added by @jtfmumm on 2025-05-06 12:21_

---

_@jtfmumm reviewed on 2025-05-06 12:41_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:267 on 2025-05-06 12:41_

This method is copied from [Zanie's PR](https://github.com/astral-sh/uv/pull/7934/files#diff-40b5f94e00ec244afb2a88a5f7c63b9e6134dc0fc813526540a14ffff7fabd7bR267-R281). This allowed me to check on Windows whether an interpreter is standalone (`is_managed` and check it's not PyPy or GraalPy). But the correct way is to patch sysconfig on Windows to directly detect standalone. I have a TODO where this is used

---

_@jtfmumm reviewed on 2025-05-06 13:35_

---

_Review comment by @jtfmumm on `.github/workflows/ci.yml`:220 on 2025-05-06 13:35_

We were using an earlier version of uv to install Python. But this breaks tests with the new install/upgrade mechanism. So we're building the binary with the latest version and using that instead. 

This is using `cargo nextest --no-run` with the same configuration as `nextest` is run below. 

---

_Closed by @jtfmumm on 2025-05-06 15:48_

---

_Reopened by @jtfmumm on 2025-05-06 15:48_

---

_@zanieb reviewed on 2025-05-06 17:04_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:220 on 2025-05-06 17:04_

I would rather not do this if we can avoid it. How does this break the tests? The tests should be isolated from how Python is installed.

---

_@jtfmumm reviewed on 2025-05-06 17:09_

---

_Review comment by @jtfmumm on `.github/workflows/ci.yml`:220 on 2025-05-06 17:09_

This affects tests that depend on our managed `python-build-standalone` installations. For example, virtual environments now point to the symlink directory/junction, which wouldn't exist with mainline uv installations. 

Once this PR is released, we could revert this if we want to. I'd be open to another approach if you think there's a better way to do it.

---

_@zanieb reviewed on 2025-05-06 17:26_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:220 on 2025-05-06 17:26_

> This affects tests that depend on our managed python-build-standalone installations. For example, virtual environments now point to the symlink directory/junction, which wouldn't exist with mainline uv installations.

What's the exact failure mode? Virtual environments point to an invalid directory? Or snapshots change? Does this mean that this change is breaking for any users on existing managed Python versions?

---

_@jtfmumm reviewed on 2025-05-06 17:56_

---

_Review comment by @jtfmumm on `.github/workflows/ci.yml`:220 on 2025-05-06 17:56_

I've been investigating this further and it looks like these failures were useful. I'm going to revert the `ci.yml` change and work on a fix. 

I marked the PR as `breaking` but it was really just supposed to be that virtual environments created with an older version of uv wouldn't transparently upgrade until recreated. 

---

_Closed by @jtfmumm on 2025-05-07 12:19_

---

_Reopened by @jtfmumm on 2025-05-07 12:19_

---

_@zanieb reviewed on 2025-05-07 13:54_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:196 on 2025-05-07 13:54_

This might need a little more commentary, I'm not sure what this means or why it's here despite the comment.

---

_@zanieb reviewed on 2025-05-07 14:15_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:147 on 2025-05-07 14:15_

There are a couple things that make this function feel weird to me

1. It's only relevant for interpreters that are managed and from PBS, but it's on the `Interpreter` type
2. It takes `base_python`, which is a property of `self`, this leaves a chance for accidental inconsistency

On (1), it feels like we might want to introduce a new type? Like `StandaloneInterpreter` that implements a `try_from(Interpreter) -> Option<Self>` so you can do

```rust
if let Some(standalone) = StandaloneInterpreter::try_from(interpreter) { ... }
```

instead of exposing this method for all interpreters?

On (2), is this a performance thing? So you don't run `find_base_python` twice in `VirtualEnvironment::create`? However, we only find it early for display purposes, I think it's probably feasible to not take this value?



---

_@zanieb reviewed on 2025-05-07 14:18_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:148 on 2025-05-07 14:18_

This is accessible as `self.implementation_name()` too, but if we're doing comparisons we should probably use the typed version, like

https://github.com/astral-sh/uv/blob/0593b967ba848909c46b4c95f4ba8cbdcba147e9/crates/uv-python/src/installation.rs#L215-L218

(we'd need to move that method into `Interpreter`)

---

_@zanieb reviewed on 2025-05-07 14:24_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:672 on 2025-05-07 14:24_

Shouldn't this be `DirectorySymlink::try_from` or similar?

---

_@zanieb reviewed on 2025-05-07 14:25_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:161 on 2025-05-07 14:25_

It seems fairly reasonable to panic here, but curious why you chose to do so rather than just `?` and return `None`.

---

_@zanieb reviewed on 2025-05-07 14:26_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-07 14:26_

Can you comment on the `bin/` difference with Windows?

Should that difference be encoded in `DirectorySymlink` instead?

---

_@zanieb reviewed on 2025-05-07 14:30_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:151 on 2025-05-07 14:30_

Why is this check needed at all when calls to this function are gated by `is_standalone()`?

---

_@zanieb reviewed on 2025-05-07 14:32_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:559 on 2025-05-07 14:32_

It'd be good to be clear here that the reason we do this is because `PYTHON_BUILD_STANDALONE` is not set in Windows build's sysconfig.

Why do inequality comparisons here instead of comparing for equality to CPython?

---

_@zanieb reviewed on 2025-05-07 14:34_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:529 on 2025-05-07 14:34_

This logic seems repeated several times now, is there a clean way for us to abstract it?

---

_@zanieb reviewed on 2025-05-07 14:39_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4660 on 2025-05-07 14:39_

We should probably be clear that this only applies to managed Python versions. Environments created with system Python versions cannot be upgraded by us (though, technically, we could implement this versioned directory for them if we really wanted — it's probably not worth it though).

We should also comment about how this affects existing virtual environments.

---

_@zanieb reviewed on 2025-05-07 14:43_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:840 on 2025-05-07 14:43_

A dedicated `StandaloneIntepreter` type might be helpful to keep the logic about the `executable` to use in the `uv-python` crate.

---

_@zanieb reviewed on 2025-05-07 14:45_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:789 on 2025-05-07 14:45_

Why is this a function? It's only used once. I also don't think it needs to be `pub`, unless I'm missing something?

---

_@zanieb reviewed on 2025-05-07 14:46_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:790 on 2025-05-07 14:46_

Why the `-dir` suffix?

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:180 on 2025-05-07 14:48_

Is there a reason this can't be performed above instead of tracking this state in a boolean?

---

_@zanieb reviewed on 2025-05-07 14:48_

---

_@zanieb reviewed on 2025-05-07 14:49_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:678 on 2025-05-07 14:49_

Can this early return? Why not `?`?

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:672 on 2025-05-07 14:50_

This should have documentation, especially about why it would return `None`.

---

_@zanieb reviewed on 2025-05-07 14:50_

---

_@zanieb reviewed on 2025-05-07 14:52_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:187 on 2025-05-07 14:52_

If this returns `None` we'll just silently not create the symlink directory? Would that break something?

---

_@zanieb reviewed on 2025-05-07 14:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1270 on 2025-05-07 14:53_

Should `symlink_exists` be exposed as a method on `DirectorySymlink`?

---

_@zanieb reviewed on 2025-05-07 15:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-07 15:00_

Can you explain what's going on here? Why do we need this special handling in `run`?

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:85 on 2025-05-07 15:12_

Why is this not just `matches_installation` for the name?

Is there a good reason to move this into a one-line function?

---

_@zanieb reviewed on 2025-05-07 15:12_

---

_@zanieb reviewed on 2025-05-07 15:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:101 on 2025-05-07 15:14_

I'm unsure of the value of these. Is this just for readability of `request.is_version_request() && !request.is_minor_request())`? I'm generally hesitant to add indirection for something only used once. 

---

_@zanieb reviewed on 2025-05-07 15:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:96 on 2025-05-07 15:16_

This could be misleading, e.g., this would not match `cpython-3.14` which is a minor version request.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:243 on 2025-05-07 15:19_

Reflecting on https://github.com/astral-sh/uv/pull/13312/files#r2077898624

Is there a reason you're taking full-blown `PythonRequest` types here? That's very permissive. I think you'd be better off being more restrictive upfront?

---

_@zanieb reviewed on 2025-05-07 15:19_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1422 on 2025-05-07 15:20_

Ah, can you say more about this design decision? I'm very surprised that you're reusing the `install` implementation here. It seems like it's going to add complexity?

---

_@zanieb reviewed on 2025-05-07 15:20_

---

_@zanieb reviewed on 2025-05-07 15:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:243 on 2025-05-07 15:21_

Ah I see, it's because of https://github.com/astral-sh/uv/pull/13312/files#r2077908733

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:981 on 2025-05-07 15:23_

What's this here for? Please call out changes to existing experiences like this.

I don't think this is true, either

```
❯ uv venv -p 3.13.2
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ cat .venv/pyvenv.cfg
home = /Users/zb/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin
implementation = CPython
uv = 0.7.2
version_info = 3.13.2
include-system-site-packages = false
prompt = uv
```

Is that something we're changing?

---

_@zanieb reviewed on 2025-05-07 15:23_

---

_@zanieb reviewed on 2025-05-07 15:24_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:981 on 2025-05-07 15:24_

tested another tool too

```
❯ uvx virtualenv .venv
Installed 4 packages in 13ms
created virtual environment CPython3.13.3.final.0-64 in 175ms
  creator CPython3Posix(dest=/Users/zb/workspace/uv/.venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, wheel=latest, via=copy, app_data_dir=/Users/zb/Library/Application Support/virtualenv)
    added seed packages: pip==25.0.1
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
❯ cat .venv/pyvenv.cfg
home = /Users/zb/.local/share/uv/python/cpython-3.13.3-macos-aarch64-none/bin
implementation = CPython
version_info = 3.13.3.final.0
...
```

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:175 on 2025-05-07 15:25_

These code blocks need to match our existing styling

---

_@zanieb reviewed on 2025-05-07 15:25_

---

_@zanieb reviewed on 2025-05-07 15:25_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:163 on 2025-05-07 15:25_

`python-build-standalone` is an implementation detail, not supposed to be user facing. I would reframe this and add more context. You might just want to call out that it's not supported for PyPy and GraalPy in an admonition. Those are not commonly used, so we can _mostly_ talk as if CPython is the only variant.

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:179 on 2025-05-07 15:28_

Related to https://github.com/astral-sh/uv/pull/13312/files#r2077914101

I don't think this is the right way to capture virtual environments using patch versions. We should track the request, e.g., `uv venv -p 3.13` vs `uv venv -p 3.13.1` should work.

---

_@zanieb reviewed on 2025-05-07 15:28_

---

_@zanieb reviewed on 2025-05-07 15:29_

---

_Review comment by @zanieb on `docs/guides/install-python.md`:123 on 2025-05-07 15:29_

We shouldn't repeat the concept documentation verbatim. The guide should be simple and the concept should have more details.

---

_@zanieb reviewed on 2025-05-07 15:33_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1492 on 2025-05-07 15:33_

Any tests that use `python install` will need to be under the `python-managed` flag

---

_@zanieb reviewed on 2025-05-07 15:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1481 on 2025-05-07 15:34_

```suggestion
    // Install a lower patch version.
```

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_install.rs`:1492 on 2025-05-08 09:04_

All tests in this module (and my new [`python_upgrade.rs` module](crates/uv/tests/it/main.rs)) are under the `python-managed` flag, unless I'm missing something.

---

_@jtfmumm reviewed on 2025-05-08 09:04_

---

_@jtfmumm reviewed on 2025-05-08 09:05_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_install.rs`:1492 on 2025-05-08 09:05_

All the tests in this module (and my new `python_upgrade` module) are under that flag, unless I'm missing something.

---

_@jtfmumm reviewed on 2025-05-08 11:07_

---

_Review comment by @jtfmumm on `crates/uv/src/lib.rs`:981 on 2025-05-08 11:07_

I've removed this behavior. 

---

_Review comment by @jtfmumm on `crates/uv-fs/src/lib.rs`:196 on 2025-05-08 11:08_

I've made it explicit that the reparse point attribute indicates this is a symlink or a junction.

---

_@jtfmumm reviewed on 2025-05-08 11:08_

---

_Closed by @jtfmumm on 2025-05-08 12:08_

---

_Reopened by @jtfmumm on 2025-05-08 12:08_

---

_Closed by @jtfmumm on 2025-05-08 12:17_

---

_Reopened by @jtfmumm on 2025-05-08 12:17_

---

_Closed by @jtfmumm on 2025-05-08 12:28_

---

_Reopened by @jtfmumm on 2025-05-08 12:28_

---

_@jtfmumm reviewed on 2025-05-08 13:47_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-08 13:47_

I can add a comment explaining that on unix we place the standalone executable in a `bin` subdirectory but not on Windows. 

I could add a method to encapsulate that on `DirectorySymlink`, something like `symlink_executable_directory()`. Not sure the best name to distinguish them.

---

_@jtfmumm reviewed on 2025-05-08 13:50_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-08 13:50_

This is in order to support transparent upgrades for virtual envs created by `uv run python -m venv`. We need to run with the symlink path in order for it to be captured when the virtual environment is created. 

---

_@jtfmumm reviewed on 2025-05-08 13:54_

---

_Review comment by @jtfmumm on `crates/uv/src/lib.rs`:1422 on 2025-05-08 13:54_

My initial approach was to try to create a separate `upgrade` implementation. But it re-uses so much of the same logic as install that it seemed like much more of a maintenance burden to duplicate it than to add some upgrade special case checks. Plus, if we do add support for `uv python install --upgrade`, it will naturally use the same code path.

---

_@zanieb reviewed on 2025-05-08 14:24_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-08 14:24_

Why do we need to put it in a different directory?

---

_@zanieb reviewed on 2025-05-08 14:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-08 14:25_

It seems critical to write a comment to that effect. It seems problematic that special-casing is needed here though, if that's necessary, won't our `~/.local/bin/python` executables never work?

---

_@zanieb reviewed on 2025-05-08 14:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1492 on 2025-05-08 14:26_

Ah okay I forgot it was at the module level 👍 thanks!

---

_@jtfmumm reviewed on 2025-05-08 14:28_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-08 14:28_

I'm not entirely sure why we do it that way, but I assume it's because it's the convention. When uv installs a standalone interpreter in Windows, it puts it in:

```
 C:\Users\user\AppData\Roaming\uv\python\cpython-3.10.17-windows-x86_64-none\python.exe
 ```
 
 and in Unix systems, it puts it in:
 
 ```
 /Users/user/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none/bin/python3.10
 ```
 
 That's what's being reflected here.

---

_@zanieb reviewed on 2025-05-08 14:30_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-08 14:30_

I'm not sure we should reflect that structure in our symlink directories unless it's actually necessary

---

_@jtfmumm reviewed on 2025-05-08 14:42_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-08 14:42_

The symlink directory points to the equivalent of `C:\Users\user\AppData\Roaming\uv\python\cpython-3.10.17-windows-x86_64-none` on Windows and `/Users/user/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none` on Unix. The `bin` is only for creating the full path to the executable.

---

_@jtfmumm reviewed on 2025-05-08 14:45_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-08 14:45_

I'll need to explore it further, but it may be necessary to use shims in `~/.local/bin` in order to support transparently upgradeable virtual environments created with the `venv` module when calling them directly.

---

_@zanieb reviewed on 2025-05-08 14:45_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-08 14:45_

Ah, I see.

You'll want to find a way to use or abstract the logic at https://github.com/astral-sh/uv/blob/878c2acdf30169af9f02bd23fe799f75916517e0/crates/uv-python/src/managed.rs#L386-L390 then?

---

_@zanieb reviewed on 2025-05-08 14:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-08 14:47_

I think we'll need a clear answer there before we can merge this implementation.

---

_@jtfmumm reviewed on 2025-05-08 14:47_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-08 14:47_

Yeah, that makes sense

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-08 14:52_

That makes sense to me

---

_@jtfmumm reviewed on 2025-05-08 14:52_

---

_@jtfmumm reviewed on 2025-05-08 15:12_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-08 15:12_

To be clear, I've done manual tests in the past using a shim, so I know that approach creates the upgradeable `.venv` structure (i.e., the correct `home` key and the correct symlinks in `.venv/bin`). For Windows, we'd be using the trampoline approach instead.

---

_Closed by @jtfmumm on 2025-05-09 13:38_

---

_Reopened by @jtfmumm on 2025-05-09 13:38_

---

_@konstin reviewed on 2025-05-12 15:39_

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:260 on 2025-05-12 15:39_

Do we want to resolve junctions/symlinks with relative paths? Not critical, but since we're already at it, we could revert this to https://github.com/astral-sh/uv/pull/5750/files#diff-969979506be03e89476feade2edebb4689a9c261f325988d3c7efc5e51de26d1L273-L277 . Otherwise, could you leave a TODO?

---

_@jtfmumm reviewed on 2025-05-12 15:46_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:260 on 2025-05-12 15:46_

I was trying to make the minimal change here in the context of this PR. I'll leave a TODO with your link

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-13 15:46_

I was able to do some further testing on this today. There appears to be no need for a shim and things should work with both unix installations and Windows trampoline-based installations into the `bin` directory.

---

_@jtfmumm reviewed on 2025-05-13 15:46_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:161 on 2025-05-15 18:08_

I decided to panic in two cases: the executable path has no file name or no parent directory. It seems clear something has gone wrong in either case.

On the other hand, I'm returning `None`in two cases. First, if on Unix the parent directory of the executable is not `bin`. Maybe I should panic in this case as well. But my thinking was that we might have encountered a quirky (non-managed) self-installation of the standalone interpreter and in that case we could (and should) just continue without the symlink directory approach. Currently on Unix I'm only checking if the interpreter is standalone. But I could easily be convinced there's a better way.

The second case is if the derived symlink path is the same as the executable path. This would happen if you passed in one of our standard symlink paths to this function. In such a case, we won't need to create a symlink directory.

---

_@jtfmumm reviewed on 2025-05-15 18:08_

---

_@jtfmumm reviewed on 2025-05-15 18:09_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:165 on 2025-05-15 18:09_

Done

---

_@jtfmumm reviewed on 2025-05-15 18:09_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:180 on 2025-05-15 18:09_

Fixed

---

_@jtfmumm reviewed on 2025-05-15 18:10_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:187 on 2025-05-15 18:10_

The new version of this is based on `DirectorySymlink` and would return `None` in the cases I describe in an earlier comment. In both cases, we don't want to create a symlink directory.

---

_Label `breaking` removed by @jtfmumm on 2025-05-15 18:26_

---

_@jtfmumm reviewed on 2025-05-15 18:28_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4660 on 2025-05-15 18:28_

Updated

---

_@jtfmumm reviewed on 2025-05-19 09:12_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1261 on 2025-05-19 09:12_

Transparently upgradeable `bin` Python installs are now implemented using symlink directories (and junctions on Windows) here: #13531.

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:254 on 2025-05-19 19:01_

```suggestion
        // NOTICE: dunce adds 5kb~
```

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:702 on 2025-05-19 19:04_

What's different about them?

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:679 on 2025-05-19 19:05_

Can you add an example of how those paths look like?

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:661 on 2025-05-19 19:05_

Can you add a docstring what this represents?

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:708 on 2025-05-19 19:33_

These should include the platform too, as some hosts allow multiple platforms (mac with rosetta, windows arm64 emulates x86_64 too, some users cross compile or use QEMU)

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:708 on 2025-05-19 19:35_

Struggling to follow along with what paths are computed how

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:734 on 2025-05-19 19:36_

Can this be merged with the unix path?

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:693 on 2025-05-19 19:37_

Where possible, we prefer `if cfg!(_)` over `#[cfg(_)]` as the former type checks and lints all branches on each host. This makes it easier to catch things locally before running them in CI.

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:738 on 2025-05-19 19:41_

nit: I usually expect `try_from` methods to return a `Result`

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:788 on 2025-05-19 19:47_

nit (since it isn't your code originally): Can we have `replace_symlink` capture the paths on error? (unix already does)

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:799 on 2025-05-19 19:49_

That home is PYTHONHOME?

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:845 on 2025-05-19 19:55_

fs_err already adds the path to the error message

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:858 on 2025-05-19 19:57_

fs_err already adds the path to the error message

---

_Review comment by @konstin on `docs/concepts/python-versions.md`:164 on 2025-05-19 20:03_

From a user perspective: What does it mean if an interpreter can or cannot be upgraded, how is that different from just uninstalling the old version and installing the new one?

---

_Review comment by @konstin on `docs/concepts/python-versions.md`:175 on 2025-05-19 20:04_

I'm not sure what the right interface is, but note that for uninstall, it needs an explicit `--all` to act on all versions

---

_@konstin reviewed on 2025-05-19 20:05_

Some preliminary review comments

---

_@jtfmumm reviewed on 2025-05-20 07:16_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:788 on 2025-05-20 07:16_

Maybe? This is a catch-all for io errors and ultimately allows us to display a targeted message. What are you imagining? 

---

_@jtfmumm reviewed on 2025-05-20 07:23_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:845 on 2025-05-20 07:23_

This function was moved almost verbatim out from a method since it needs to be used outside that context as well now. I haven't changed the error handling. 

---

_@jtfmumm reviewed on 2025-05-20 07:24_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:845 on 2025-05-20 07:24_

If we should change the handling, let's move that out of this PR to reduce variables.

---

_@jtfmumm reviewed on 2025-05-20 07:24_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:858 on 2025-05-20 07:24_

See last comment.

---

_@jtfmumm reviewed on 2025-05-20 07:27_

---

_Review comment by @jtfmumm on `docs/concepts/python-versions.md`:164 on 2025-05-20 07:27_

Good point, and actually this is misleading. It's just that transparent virtual environment patch upgrades aren't supported for them. Updated

---

_@jtfmumm reviewed on 2025-05-20 07:43_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:702 on 2025-05-20 07:43_

One problem is that you can have them installed in parallel with CPython standalone. Does upgrade mean to move to the highest patch, regardless of implementation? There are other questions like these as well and when discussing during design we decided that because of limited support in general, it made more sense to restrict transparent upgrades to our standalone interpreters.

---

_@jtfmumm reviewed on 2025-05-20 07:43_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:679 on 2025-05-20 07:43_

Added

---

_@jtfmumm reviewed on 2025-05-20 07:43_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:661 on 2025-05-20 07:43_

Added

---

_@jtfmumm reviewed on 2025-05-20 07:43_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:708 on 2025-05-20 07:43_

Added further documentation and the examples you requested

---

_@jtfmumm reviewed on 2025-05-20 07:44_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:734 on 2025-05-20 07:44_

Done

---

_@jtfmumm reviewed on 2025-05-20 07:47_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:799 on 2025-05-20 07:47_

Added a comment. This is from a home directory of an installation (which you pass in).

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:178 on 2025-05-20 18:39_

Can you add a new constructor that doesn't go through string serialization but uses the version directly? I think we're also losing the Python variant here. (Can be a split out PR)

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:189 on 2025-05-20 18:42_

How real is this error path, could a python interpreter go away in the meantime (e.g. different downloads json or we drop support for a Python version), what's the behavior we want for that?

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:236 on 2025-05-20 18:45_

For me, if I said `uv python upgrade 3.13.2`, I would expect to upgrade that Python version to the latest patch version, especially if I have multiple 3.13's. (Not blocking, doesn't affect most users)

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:306 on 2025-05-20 18:45_

Why is the condition different for upgrade?

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:453 on 2025-05-20 18:54_

nit: We usually use the entry api for combined get-and-insert

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:957 on 2025-05-20 19:32_

https://github.com/astral-sh/uv/pull/13561

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:708 on 2025-05-20 19:40_

We can skip on this for now, but I think we do need it as a follow-up, also given https://github.com/astral-sh/uv/pull/13312#discussion_r2097225740

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:788 on 2025-05-20 19:42_

Usually, we have the innermost (IO) error attach the paths, so that anything higher level doesn't have to show the path involved, but can focus on the higher-level feature that failed. Currently it repeats the path for Unix, making the error verbose.

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:799 on 2025-05-20 19:44_

"home directory" is a confusing term, since it's generally used for the user home, while `PYTHONHOME` is a not well known detail of CPython.

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:845 on 2025-05-20 19:44_

Can you pull this out in a separate PR? (In the hope that it's a small PR and small rebase)

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:1457 on 2025-05-20 19:46_

Can you merge this and the previous test? Usually I'm all for small tests, but Python downloads and installs are slow so reuse helps the test speed

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:1821 on 2025-05-20 19:51_

nice!

---

_Review comment by @konstin on `crates/uv/tests/it/python_upgrade.rs`:94 on 2025-05-20 19:57_

Do we pin this patch version somewhere? Otherwise note we need to bump those snapshots for Python patch version we add.

---

_Review comment by @konstin on `crates/uv/tests/it/python_upgrade.rs`:186 on 2025-05-20 19:59_

Can you merge this test with the one above? Again so that we reduce the number of Python installs where easily feasible

---

_@konstin reviewed on 2025-05-20 20:09_

---

_@jtfmumm reviewed on 2025-05-21 08:46_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:799 on 2025-05-21 08:46_

Renaming to "base"

---

_@jtfmumm reviewed on 2025-05-21 08:51_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:236 on 2025-05-21 08:51_

What would it mean to upgrade `3.13.2` to the latest patch but not the other `3.13`s? Upgrading installs the latest patch and transparently upgrades all virtual environments on that minor version.

---

_@jtfmumm reviewed on 2025-05-21 09:07_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_upgrade.rs`:94 on 2025-05-21 09:07_

Filtering

---

_@jtfmumm reviewed on 2025-05-21 09:53_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_upgrade.rs`:186 on 2025-05-21 09:53_

Done

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_upgrade.rs`:94 on 2025-05-21 09:53_

Filtered

---

_@jtfmumm reviewed on 2025-05-21 09:53_

---

_@jtfmumm reviewed on 2025-05-21 09:53_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_install.rs`:1457 on 2025-05-21 09:53_

Done

---

_@jtfmumm reviewed on 2025-05-21 09:54_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:453 on 2025-05-21 09:54_

Updated

---

_@jtfmumm reviewed on 2025-05-21 09:55_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:306 on 2025-05-21 09:55_

I've added a comment explaining this. `request.download` is the highest patch for that minor version. We need to check for an exact match or otherwise we need to install it for an upgrade.

---

_@jtfmumm reviewed on 2025-05-21 09:55_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:178 on 2025-05-21 09:55_

Updated

---

_@jtfmumm reviewed on 2025-05-21 10:04_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:189 on 2025-05-21 10:04_

That's a good point. I've updated this code to only insert an `InstallRequest` when it can be constructed. 

---

_@jtfmumm reviewed on 2025-05-21 10:08_

---

_Review comment by @jtfmumm on `docs/guides/install-python.md`:123 on 2025-05-21 10:08_

Updated

---

_@jtfmumm reviewed on 2025-05-21 10:15_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:788 on 2025-05-21 10:15_

Yeah that makes sense. I think we can do that and it will simplify a number of existing custom error types we have. I can do that as a separate PR once this one is merged.

---

_Assigned to @zanieb by @zanieb on 2025-05-21 15:15_

---

_Comment by @konstin on 2025-05-21 19:55_

I tried to break it and failed :)

---

_@konstin approved on 2025-05-21 19:55_

---

_@konstin reviewed on 2025-05-21 20:11_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:467 on 2025-05-21 20:11_

Does this handle prereleases and updates from prerelease to stable correctly?

---

_@jtfmumm reviewed on 2025-05-22 07:59_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:467 on 2025-05-22 07:59_

I don't think it's possible to add an automated test for this because we don't keep around outdated prereleases in new versions of uv. But in manual testing of 3.14 prereleases, transparent upgrades work as expected. 

For example:
```
uv python install 3.14.0a3
uv venv -p 3.14
uv run python --version # 3.14.0a3
uv python upgrade 3.14 # --or-- uv python install 3.14.0a7
uv run python --version # 3.14.0a7
```
This also works as expected if you install the lower prerelease second. Of course, I can't test 3.14.1 at the moment, but that upgrade will work because the patch is higher.

This code you're commenting on does rely on the ordering imposed by `installations.find_all()`. I could add a comment to that effect or I could do

```
        minor_versions
            .entry(minor_version)
            .and_modify(|high_installation: &mut ManagedPythonInstallation| {
                if installation.key() >= high_installation.key() {
                    *high_installation = installation.clone()
                }
            })
            .or_insert(installation);
```

which adds a few more clones but still works. 

---

_@konstin reviewed on 2025-05-22 18:46_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:467 on 2025-05-22 18:46_

Either is fine by me, FWIW there is no noticeable cost to cloning here.

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:467 on 2025-05-23 07:48_

Ok, I've updated to use the installation key instead of the patch.

---

_@jtfmumm reviewed on 2025-05-23 07:48_

---

_@zanieb reviewed on 2025-05-27 22:34_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4986 on 2025-05-27 22:34_

What does this mean in the context of an upgrade?

---

_@zanieb reviewed on 2025-05-27 22:34_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4972 on 2025-05-27 22:34_

Does this make sense here? Should the doc be updated?

---

_@zanieb reviewed on 2025-05-27 22:42_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4731 on 2025-05-27 22:42_

```suggestion
    /// Upgrade installed Python versions to the latest supported patch release.
    ///
    /// A target Python minor version to upgrade may be provided, e.g., `3.13`. Multiple versions
    /// may be provided to perform more than one upgrade.
    ///
    /// If no target version is provided, then uv will attempt to upgrade all managed CPython
    /// versions.
    ///
    /// When an upgrade is performed, virtual environments created by uv will automatically
    /// use the new version. However, if the virtual environment was created before the
    /// upgrade functionality was added, it will continue to use the old Python version; to enable
    /// upgrades, the environment must be recreated.
    ///
    /// Upgrades are not yet supported for alternative implementations, like PyPy.
```

---

_@zanieb reviewed on 2025-05-27 22:44_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4753 on 2025-05-27 22:44_

What happens if there are multiple patch versions for a given minor, e.g., if you do `uv python install 3.13.1 3.13.2 && uv python upgrade 3.13`? Do we always select `3.13.2` for upgrade? Can you document the behavior?

---

_@zanieb reviewed on 2025-05-27 22:44_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4931 on 2025-05-27 22:44_

During upgrade, this is the directory to look for installations in, right?

---

_@zanieb reviewed on 2025-05-27 22:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4936 on 2025-05-27 22:45_

I'm not sure we need to say "attempt" or "that has been installed here", I think those are both implied.

Copying my suggestion from above:

> If no target version is provided, then uv will attempt to upgrade all managed CPython versions.

---

_@zanieb reviewed on 2025-05-27 22:46_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4959 on 2025-05-27 22:46_

How will this interface work in the future if / when we add support for upgrading other implementations, like PyPy?

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4753 on 2025-05-27 22:50_

Looks like that is the behavior per https://github.com/astral-sh/uv/pull/13312/files#diff-edf4feffb3da2fa558fbdc0886240f421dcbcdbc722f09573e4dd86f75bad619R185-R189

So... 

> If multiple patch versions are installed from a given minor, uv will always upgrade the latest patch version.


---

_@zanieb reviewed on 2025-05-27 22:50_

---

_@zanieb reviewed on 2025-05-27 22:56_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:702 on 2025-05-27 22:56_

I do briefly want to discuss this per https://github.com/astral-sh/uv/pull/13312/files#r2110434395

My understanding of the design is that we wanted to start with a limited scope, but I would expect to support upgrading PyPy in the long-term. Do you think that's infeasible? I would like to ensure this design doesn't limit us from doing so.

---

_@zanieb reviewed on 2025-05-27 22:58_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:708 on 2025-05-27 22:58_

I'm not sure we can skip on it. Won't we break compatibility with older versions if we change this name?

It seems safest to use the full key here, minus the patch version?

Related

- #13474 
- #11625
- https://github.com/astral-sh/uv/pull/13475
- #9788



---

_@zanieb reviewed on 2025-05-27 23:00_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:718 on 2025-05-27 23:00_

Just wondering, what is this for? When isn't `python` in `bin/`?

---

_@zanieb reviewed on 2025-05-27 23:01_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:718 on 2025-05-27 23:01_

Oh, nevermind — I got confused here.

---

_@zanieb reviewed on 2025-05-27 23:02_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:729 on 2025-05-27 23:02_

I'm not sure if these comments are needed since they're present on `Self`

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:731 on 2025-05-27 23:02_

A comment on why we return here would be helpful.

---

_@zanieb reviewed on 2025-05-27 23:02_

---

_@jtfmumm reviewed on 2025-05-27 23:03_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:702 on 2025-05-27 23:03_

I do think it's feasible. It's just a question of how we want to approach it. Do we want `uv python upgrade 3.13` to mean upgrade CPython but `uv python upgrade pypy3.13` to mean upgrade PyPy? That's one possibility. I don't think the implementation itself will be difficult.

---

_@jtfmumm reviewed on 2025-05-27 23:06_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:708 on 2025-05-27 23:06_

Yeah, I can make the change in the scope of this PR if we think it's important. It could be reasonable to use the full key minus the patch, though the prerelease is more nuanced. On the follow-up `--preview` upgrade PR I add the variant. I can create a small PR off this one to add support for the rest.

---

_@zanieb reviewed on 2025-05-27 23:07_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:692 on 2025-05-27 23:07_

I think it's common for us to just write `from(...) -> Option<...>`, I don't think you need to encode the `maybe` in the name when the type system captures that.

---

_@jtfmumm reviewed on 2025-05-27 23:08_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4753 on 2025-05-27 23:08_

When upgrading, we install the latest available patch if it has not already been installed and leave the other patches in place. I can add more documentation.

---

_@zanieb reviewed on 2025-05-27 23:12_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:747 on 2025-05-27 23:12_

I was curious why this wasn't the only constructor, it looks like we only need the other `maybe_from` for the case where we have a `ManagedPythonInstallation` and don't want to query the interpreter to construct a proper `Interpreter`?

---

_@jtfmumm reviewed on 2025-05-27 23:13_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:692 on 2025-05-27 23:13_

I opted for `maybe_from()` because `from()` would make me think there was a `From` implementation returning `T` (rather than `Option<T>`). But if that's how we do it, I can update it.

---

_@jtfmumm reviewed on 2025-05-27 23:14_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:747 on 2025-05-27 23:14_

That's right

---

_@zanieb reviewed on 2025-05-27 23:15_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:84 on 2025-05-27 23:15_

cc @BurntSushi on whether or not we want to document this safety more thoroughly.

---

_@jtfmumm reviewed on 2025-05-27 23:15_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4959 on 2025-05-27 23:15_

I think this is an open question. One possibility is `pypy3.13` for upgrading PyPy.

---

_@jtfmumm reviewed on 2025-05-27 23:16_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4936 on 2025-05-27 23:16_

Do you want me to remove "attempt" from your suggestion as well?

---

_@zanieb reviewed on 2025-05-27 23:16_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:257 on 2025-05-27 23:16_

I don't quite follow this TODO

---

_@zanieb reviewed on 2025-05-27 23:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/environment.rs`:79 on 2025-05-27 23:25_

Is this safe? Can't the interpreter path change other behaviors? 

---

_@zanieb reviewed on 2025-05-27 23:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/environment.rs`:79 on 2025-05-27 23:26_

I think we explicitly avoid canonicalizing elsewhere, e.g., `Interpreter::query_cached`. I think you may need to use your symlink directory detection to only canonicalize when safe.

---

_@zanieb reviewed on 2025-05-27 23:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-27 23:27_

Maybe a dumb question, but.. why isn't `sys_executable` the executable in the symlink directory?

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:236 on 2025-05-27 23:29_

I also had this question at https://github.com/astral-sh/uv/pull/13312/files#r2110432877 so I'll re-open to make sure we're on the same page.

What if I do `uv python install 3.13.1 && uv venv -p 3.13 && uv python install 3.13.2`?

Is my virtual environment transparently upgraded?

---

_@zanieb reviewed on 2025-05-27 23:29_

---

_@zanieb reviewed on 2025-05-27 23:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:473 on 2025-05-27 23:31_

```suggestion
                    "All requested versions already on latest supported patch release"
```

This may be confusing otherwise due to uv bundling supported patch versions.

---

_@zanieb reviewed on 2025-05-27 23:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-27 23:32_

Up here we use this to check if someone actually requested something so we don't say "All requested" when the default request is used, does that apply during upgrades? Do we need a special case message for that?

---

_@jtfmumm reviewed on 2025-05-27 23:33_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-27 23:33_

For a specific interpreter, that would potentially be inaccurate. The symlink directory-containing executable path only points to the highest patch version. So I don't know if it makes sense to have that on `Interpreter` since an interpreter doesn't know anything about other interpreters.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:220 on 2025-05-27 23:33_

Just noting this is where my review is stopping — it's late, I'll pick it back up here tomorrow.

---

_@zanieb reviewed on 2025-05-27 23:33_

---

_@zanieb reviewed on 2025-05-27 23:35_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4936 on 2025-05-27 23:35_

Haha oops, I think so? I worry it's just added complexity.

---

_@zanieb reviewed on 2025-05-27 23:36_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4753 on 2025-05-27 23:36_

👍 seems important to note that the previous patches will not be removed.

---

_@zanieb reviewed on 2025-05-27 23:36_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4959 on 2025-05-27 23:36_

It's definitely supported in the `PythonRequest` syntax / seems fairly reasonable to me.

---

_@zanieb reviewed on 2025-05-27 23:37_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:692 on 2025-05-27 23:37_

Agree that's a little confusing, but we do it pretty often and I'd prioritize consistency with the rest of the codebase.

---

_@zanieb reviewed on 2025-05-27 23:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-27 23:39_

Ah I'm not asking why the logic isn't in `Interpeter`, I'm asking why the interpreter itself isn't reporting `sys.executable` as the file in the symlink directory. 

---

_@zanieb reviewed on 2025-05-27 23:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-27 23:42_

I am just confused that we need to special-case `uv run`. Doesn't `python -m venv` work by itself?

---

_@jtfmumm reviewed on 2025-05-28 00:18_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-28 00:18_

So `sys.executable` will give the real/resolved path to the executable in the symlink directory. The `venv` module uses the actual path it was called with to populate the `pyvenv.cfg:home` key and to determine the target path for the symlink in `.venv/bin` (on Unix). So this special-casing is ensuring we call it with the symlink directory path instead of the fully resolved one. That's necessary for it to be transparently upgradeable.

---

_@jtfmumm reviewed on 2025-05-28 00:19_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-28 00:19_

I can add more documentation about this here.

---

_@zanieb reviewed on 2025-05-28 00:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-28 00:31_

Is the key here is that `Interpreter` for these managed installations is being queried from the resolved directory instead of the symlinked one?

---

_@jtfmumm reviewed on 2025-05-28 12:21_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4931 on 2025-05-28 12:21_

Yes

---

_@jtfmumm reviewed on 2025-05-28 12:27_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:692 on 2025-05-28 12:27_

I changed `maybe_from` to `from_executable` and `maybe_from_interpreter` to `from_interpreter`. That avoids the `From` ambiguity.

---

_@jtfmumm reviewed on 2025-05-28 12:33_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:257 on 2025-05-28 12:33_

In this code, if this is a trampoline `Script` or a relative path, we call `dunce::canonicalize`. On `main` this was true, but we also called it for absolute paths that were `Python` trampolines (because we always called it). So this TODO is a path to avoid resolving symlinks and junctions in all cases. 

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-28 12:42_

What matters is what command we are running when we actually run `<python-command> -m venv`.

`/path/to/<symlink-dir>/bin/python3.10` will create a virtual environment that uses `/path/to/<symlink-dir>/bin/python3.10` and will transparently upgrade when the symlink directory is pointed to a higher patch version.

`/path/to/cpython-3.10.8-macos-aarch64-none/bin/python3.10` will create a virtual environment that uses `/path/to/cpython-3.10.8-macos-aarch64-none/bin/python3.10` and thus won't transparently upgrade when the symlink directory is updated since it doesn't use the symlink directory at all.

---

_@jtfmumm reviewed on 2025-05-28 12:42_

---

_@jtfmumm reviewed on 2025-05-28 12:52_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4986 on 2025-05-28 12:52_

Say you did `uv python install 3.10.8 --default --preview`, creating `python`, `python3`, and `python3.10` in `bin`. `uv python upgrade 3.10 --default --preview` would upgrade all to 3.10.17. However, this should probably go away with the `bin` changes (in the other PR) since transparent upgrades handle this for you. 

I'll remove it here to reduce noise in the review.

---

_@jtfmumm reviewed on 2025-05-28 12:52_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4972 on 2025-05-28 12:52_

Thinking more about it, I think I should just remove `--force` from upgrade. 

---

_@jtfmumm reviewed on 2025-05-28 12:59_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 12:59_

I've removed `default` as an upgrade option, so this shouldn't be relevant anymore.

---

_@jtfmumm reviewed on 2025-05-28 13:02_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4972 on 2025-05-28 13:02_

Removed

---

_@jtfmumm reviewed on 2025-05-28 13:21_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4753 on 2025-05-28 13:21_

Added

---

_@zanieb reviewed on 2025-05-28 14:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1273 on 2025-05-28 14:15_

So the question is: why is the user ever invoking `/path/to/cpython-3.10.8-macos-aarch64-none/bin/python3.10`?

That path should be purely internal. I presume it's being constructed from `ManagedPythonInstallation`, which hints to me that we should be adjusting how we construct `Interpreter`s from `ManagedPythonInstallation` instead of special-casing this in `uv run`. Otherwise, we're going to have to implement this repeatedly.

---

_@zanieb reviewed on 2025-05-28 14:16_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4931 on 2025-05-28 14:16_

Should we change the doc then?

---

_@jtfmumm reviewed on 2025-05-28 14:28_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4931 on 2025-05-28 14:28_

I can change it to `The directory to discover and store Python installations in.` It's also the directory upgrade will install to.

---

_@zanieb reviewed on 2025-05-28 14:57_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:163 on 2025-05-28 14:57_

Can you update this to use the latest language from the CLI?

---

_@zanieb reviewed on 2025-05-28 14:57_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:181 on 2025-05-28 14:57_

We stylize this with a trailing `:`

---

_@zanieb reviewed on 2025-05-28 14:57_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:183 on 2025-05-28 14:57_

We capitalize list items and end with a period.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:265 on 2025-05-28 14:58_

This logic feels like it should be abstracted out of here.

---

_@zanieb reviewed on 2025-05-28 14:58_

---

_@zanieb reviewed on 2025-05-28 14:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:265 on 2025-05-28 14:59_

Related to https://github.com/astral-sh/uv/pull/13312/files#r2112036078

---

_@zanieb reviewed on 2025-05-28 14:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 14:59_

Can you explain? `is_default_install` means a bare `uv python install` and I think you support a bare `uv python upgrade`.

---

_@zanieb reviewed on 2025-05-28 15:01_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4731 on 2025-05-28 15:01_

I think this is kind of confusing as written

```suggestion
    /// During an upgrade, uv will not uninstall outdated patch versions.
```

---

_@jtfmumm reviewed on 2025-05-28 15:11_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 15:11_

The behavior is different though. For a bare `uv python upgrade`, we upgrade all installed versions. If they are already up-to-date, then there's a message to that effect. If there are none installed, there is currently no message actually, so we could add something like "No installed versions to upgrade".

---

_@zanieb reviewed on 2025-05-28 15:21_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4931 on 2025-05-28 15:21_

Perhaps "The directory Python installations are stored in"?

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 15:23_

Is the message

>  "All requested versions already on latest supported patch release"

If so, that's the case I'm talking about — where if you haven't requested any versions we shouldn't say "requested versions".

> If there are none installed, there is currently no message actually, so we could add something like "No installed versions to upgrade".

Yes, this case is important too.


---

_@zanieb reviewed on 2025-05-28 15:23_

---

_@jtfmumm reviewed on 2025-05-28 17:51_

---

_Review comment by @jtfmumm on `docs/concepts/python-versions.md`:163 on 2025-05-28 17:51_

Updated

---

_@jtfmumm reviewed on 2025-05-28 18:16_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 18:16_

Yes that's right. I've added a message for the other case: "There are no installed versions to upgrade"

---

_@zanieb reviewed on 2025-05-28 18:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 18:50_

This still looks wrong in the case I originally pointed out:

```
❯ uv python upgrade
Installed 5 versions in 6.85s
 + cpython-3.9.22-macos-aarch64-none
 + cpython-3.10.17-macos-aarch64-none
 + cpython-3.11.12-macos-aarch64-none
 + cpython-3.12.10-macos-aarch64-none
 + cpython-3.14.0a6-macos-aarch64-none
❯ uv python upgrade
All requested versions already on latest supported patch release
```

The user has not requested any versions, we should have a different message.

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 19:08_

Ah I see what you mean. I'll fix that. Something like `All versions already on latest supported patch release`

---

_@jtfmumm reviewed on 2025-05-28 19:08_

---

_@jtfmumm reviewed on 2025-05-28 19:54_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:484 on 2025-05-28 19:54_

Added. I also added a message for the case where you run `uv python upgrade` but there are no versions installed: `There are no installed versions to upgrade`

---

_@jtfmumm reviewed on 2025-05-28 23:03_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:708 on 2025-05-28 23:03_

Opened the PR against this one (#13712).

---

_@BurntSushi reviewed on 2025-05-29 12:36_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:84 on 2025-05-29 12:36_

I would just add `SAFETY: ` as a prefix (as a convention for all such cases of discharging proof obligations), but otherwise I think this is sufficient as-is. It's a stable guarantee provided by `std` that is explicitly mentioned in the docs of `std::env::set_var`. So this seems okay. With that said, maybe an improvement here would be:

```rust
// SAFETY: `std::env::set_var` is safe to call on Windows, and
// this code only ever runs on Windows.
```

---

_@jtfmumm reviewed on 2025-05-29 15:07_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:84 on 2025-05-29 15:07_

As a separate question, there are numerous existing instances of `unsafe` in this file on `main` that do not have a `SAFETY` description. Should I open an issue to update those?

---

_@BurntSushi reviewed on 2025-05-29 15:11_

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:84 on 2025-05-29 15:11_

Ideally yeah. IIRC, this crate is something we vendored from another source (I can't remember where at the moment) that used `unsafe` as a means to keep the binary size very small (by avoiding panicking branches). IDK if that motivation is still relevant or not. If not, perhaps we could remove many uses of `unsafe`.

But yeah ideally we'd follow this blueprint: https://jack.wrenn.fyi/blog/safety-hygiene/

---

_@jtfmumm reviewed on 2025-05-30 19:24_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:236 on 2025-05-30 19:24_

Yes. There are snapshot tests for this.

---

_Comment by @codspeed-hq[bot] on 2025-05-30 23:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Fpython-upgrade)

### Merging #13312 will **improve performances by 17.89%**

<sub>Comparing <code>jtfm/python-upgrade</code> (b817dc1) with <code>main</code> (f20a25f)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 112 ns | 95 ns | +17.89% |


---

_Review comment by @konstin on `docs/concepts/python-versions.md`:181 on 2025-06-02 10:33_

Can you make the distinction between which venvs are updated and when they aren't update, so that users know how to control this feature, maybe plus a sentence that explain the mechanism in the background?

---

_@konstin reviewed on 2025-06-02 10:34_

---

_@jtfmumm reviewed on 2025-06-03 12:48_

---

_Review comment by @jtfmumm on `docs/concepts/python-versions.md`:181 on 2025-06-03 12:48_

I've updated the docs.

---

_Comment by @jtfmumm on 2025-06-10 17:03_

I'm merging this and the rest of the PR stack into `feature/transparent-python-upgrades`. I'll open a new PR from that branch for the final review.

---

_Merged by @jtfmumm on 2025-06-10 17:04_

---

_Closed by @jtfmumm on 2025-06-10 17:04_

---

_Branch deleted on 2025-06-10 17:04_

---
