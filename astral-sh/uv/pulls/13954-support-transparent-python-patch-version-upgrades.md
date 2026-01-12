```yaml
number: 13954
title: Support transparent Python patch version upgrades
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: feature/transparent-python-upgrades
created_at: 2025-06-10T19:47:38Z
updated_at: 2025-06-21T09:22:27Z
url: https://github.com/astral-sh/uv/pull/13954
synced_at: 2026-01-12T16:10:56Z
```

# Support transparent Python patch version upgrades

---

_@jtfmumm_

> NOTE: The PRs that were merged into this feature branch have all been independently reviewed. But it's also useful to see all of the changes in their final form. I've added comments to significant changes throughout the PR to aid discussion.

This PR introduces transparent Python version upgrades to uv, allowing for a smoother experience when upgrading to new patch versions. Previously, upgrading Python patch versions required manual updates to each virtual environment. Now, virtual environments can transparently upgrade to newer patch versions.

Due to significant changes in how uv installs and executes managed Python executables, this functionality is initially available behind a `--preview` flag. Once an installation has been made upgradeable through `--preview`, subsequent operations (like `uv venv -p 3.10` or patch upgrades) will work without requiring the flag again. This is accomplished by checking for the existence of a minor version symlink directory (or junction on Windows).

### Features

* New `uv python upgrade` command to upgrade installed Python versions to the latest available patch release:
``` 
# Upgrade specific minor version 
uv python upgrade 3.12 --preview
# Upgrade all installed minor versions
uv python upgrade --preview
```
* Transparent upgrades also occur when installing newer patch versions: 
```
uv python install 3.10.8 --preview
# Automatically upgrades existing 3.10 environments
uv python install 3.10.18
```
* Support for transparently upgradeable Python `bin` installations via `--preview` flag
```
uv python install 3.13 --preview
# Automatically upgrades the `bin` installation if there is a newer patch version available
uv python upgrade 3.13 --preview
```
* Virtual environments can still be tied to a patch version if desired (ignoring patch upgrades):
```
uv venv -p 3.10.8
```

### Implementation

Transparent upgrades are implemented using:
* Minor version symlink directories (Unix) or junctions (Windows)
* On Windows, trampolines simulate paths with junctions
* Symlink directory naming follows Python build standalone format: e.g., `cpython-3.10-macos-aarch64-none`
* Upgrades are scoped to the minor version key (as represented in the naming format: implementation-minor version+variant-os-arch-libc)
* If the context does not provide a patch version request and the interpreter is from a managed CPython installation, the `Interpreter` used by `uv python run` will use the full symlink directory executable path when available, enabling transparently upgradeable environments created with the `venv` module (`uv run python -m venv`)

New types:
* `PythonMinorVersionLink`: in a sense, the core type for this PR, this is a representation of a minor version symlink directory (or junction on Windows) that points to the highest installed managed CPython patch version for a minor version key.
* `PythonInstallationMinorVersionKey`: provides a view into a `PythonInstallationKey` that excludes the patch and prerelease. This is used for grouping installations by minor version key  (e.g., to find the highest available patch installation for that minor version key) and for minor version directory naming.

### Compatibility

* Supports virtual environments created with:
  * `uv venv`
  * `uv run python -m venv` (using managed Python that was installed or upgraded with `--preview`)
  * Virtual environments created within these environments
* Existing virtual environments from before these changes continue to work but aren't transparently upgradeable without being recreated
* Supports both standard Python (`python3.10`) and freethreaded Python (`python3.10t`)
* Support for transparently upgrades  is currently only available for managed CPython installations

Closes #7287
Closes #7325
Closes #7892
Closes #9031
Closes #12977

---

_@jtfmumm reviewed on 2025-06-10 19:51_

---

_Review comment by @jtfmumm on `crates/uv-python/src/discovery.rs`:347 on 2025-06-10 19:51_

When deriving an executable from an installation, if the version is not a patch and it's a CPython installation, uv looks for a minor version symlink directory. If one is found, it will use the symlink-directory containing path. This enables transparently upgradeable virtual environments created with the `venv` module.

---

_@jtfmumm reviewed on 2025-06-10 19:55_

---

_Review comment by @jtfmumm on `crates/uv-python/src/installation.rs`:206 on 2025-06-10 19:55_

uv checks for the highest patch for each minor version key and ensures that the corresponding minor version link exists and points to it (but a symlink directory is only initially created in preview mode).

---

_@jtfmumm reviewed on 2025-06-10 19:56_

---

_Review comment by @jtfmumm on `crates/uv-python/src/installation.rs`:531 on 2025-06-10 19:56_

Used for grouping by the full key minus the patch and prerelease. Also used for deriving symlink directory names.

---

_@jtfmumm reviewed on 2025-06-10 19:58_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:638 on 2025-06-10 19:58_

This has been moved from `uv_fs` because it now knows about trampolines. If the path is to a trampoline, this will canonicalize the trampoline's internal Python path. If not, it will behave as before.

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:675 on 2025-06-10 20:02_

In a sense, the core of the implementation. We use this to derive a symlink directory name from a full Python installation path, to create such a directory (or junction on Windows), and to check for the existence of such a directory. This is also where the preview gating happens (EDIT: if preview is disabled and the symlink doesn't exist already, the constructor will return `None`).

---

_@jtfmumm reviewed on 2025-06-10 20:02_

---

_@jtfmumm reviewed on 2025-06-10 20:05_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:81 on 2025-06-10 20:05_

Updates to the trampoline logic to set the `__PYVENV_LAUNCHER__` env var for a Python launcher (the approach taken by CPython for Python Launchers) and, if it's not a virtual environment and the `PYTHONHOME` is not set, to set the `PYTHONHOME` env var to ensure the correct directories are added to `sys.path` when running with a junction trampoline.

---

_@jtfmumm reviewed on 2025-06-10 20:07_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:284 on 2025-06-10 20:07_

This update is required so we don't resolve junctions for junction trampolines. We need to be able to use the junction to ensure transparent upgrades.

---

_@jtfmumm reviewed on 2025-06-10 20:10_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:150 on 2025-06-10 20:10_

If this is an upgradeable request, we'll try to use a Python minor version link when creating the virtual environment (creating the symlink directory/junction if it doesn't exist). 

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:188 on 2025-06-10 20:12_

If this is an upgrade with empty targets, uv collects minor version requests.

---

_@jtfmumm reviewed on 2025-06-10 20:12_

---

_@jtfmumm reviewed on 2025-06-10 20:14_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:490 on 2025-06-10 20:14_

Ensure the minor version links exist for the highest patch installations per minor version key.

---

_@jtfmumm reviewed on 2025-06-10 20:15_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:655 on 2025-06-10 20:15_

When creating `bin` links, we now try to use a symlink directory as the `bin` link target if we can and this is an upgradeable request.

---

_@jtfmumm reviewed on 2025-06-10 20:17_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/uninstall.rs`:248 on 2025-06-10 20:17_

Here's where we uninstall symlink directories if there are no installations remaining for the corresponding minor version key.

---

_@jtfmumm reviewed on 2025-06-10 20:18_

---

_Review comment by @jtfmumm on `crates/uv/src/lib.rs`:1424 on 2025-06-10 20:18_

I had initially tried creating a new `python_upgrade` command, but there is so much shared machinery that it seemed like an unnecessary amount of maintenance overhead to split the code paths. Sharing them also makes adding `python install --upgrade` trivial if we decide to go that route.

---

_Marked ready for review by @jtfmumm on 2025-06-10 20:27_

---

_Review requested from @zanieb by @jtfmumm on 2025-06-10 21:00_

---

_Review requested from @konstin by @jtfmumm on 2025-06-10 21:00_

---

_@zanieb reviewed on 2025-06-11 22:42_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:346 on 2025-06-11 22:42_

`PythonMinorVersionLink::from_installation` will already return `None` on alternative implementations, and we can use `bool::then` to avoid repeating the default case here.

See https://github.com/astral-sh/uv/pull/13980

---

_@zanieb reviewed on 2025-06-11 22:47_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:338 on 2025-06-11 22:47_

Do we need preview mode to be threaded through here? Don't we always want to discover minor version links if they exist, regardless of whether preview is enabled?

---

_@zanieb reviewed on 2025-06-11 22:50_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:644 on 2025-06-11 22:50_

Not a part of this pull request, but we should follow up on it.

I'm still confused by this, why must the path be absolute? That isn't documented for this function. I think I asked this in a previous pull request.

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:156 on 2025-06-11 22:51_

I think it's okay not to gate with this, we don't tend to do that unless we're performing a particularly expensive operation.

---

_@zanieb reviewed on 2025-06-11 22:51_

---

_@jtfmumm reviewed on 2025-06-11 23:50_

---

_Review comment by @jtfmumm on `crates/uv-python/src/discovery.rs`:338 on 2025-06-11 23:50_

Even when preview is disabled, `PythonMinorVersionLink` will check if the symlink exists. We could pass `PreviewMode::Disabled` directly into `PythonMinorVersionLink::from_installation` here, but I think that's too prone to error. My goal was to centralize the logic around how to handle preview states.

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:644 on 2025-06-11 23:51_

Yeah we can follow up on it. This was a pre-existing invariant and my changes didn't eliminate the cases it would have applied to. 

---

_@jtfmumm reviewed on 2025-06-11 23:51_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:748 on 2025-06-11 23:58_

This nesting is left over from an earlier version and I'm going to consolidate into
```
if preview.is_disabled() && !minor_version_link.symlink_exists() {
    return None;
}
```

---

_@jtfmumm reviewed on 2025-06-11 23:58_

---

_@jtfmumm reviewed on 2025-06-11 23:59_

---

_Review comment by @jtfmumm on `crates/uv-python/src/discovery.rs`:346 on 2025-06-11 23:59_

Merged

---

_@zanieb reviewed on 2025-06-12 19:50_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:338 on 2025-06-12 19:50_

I see, I misread that code.

I'm a bit confused by it still, but I'll read that more carefully and see if I have questions.

---

_@zanieb reviewed on 2025-06-12 19:55_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:62 on 2025-06-12 19:55_

Should this be "for Python minor version link at"?

---

_@zanieb reviewed on 2025-06-12 19:55_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:78 on 2025-06-12 19:55_

As in https://github.com/astral-sh/uv/pull/13954/files#r2143547453, should this be "Python minor version link"?

---

_@zanieb reviewed on 2025-06-12 19:56_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:828 on 2025-06-12 19:56_

Does this need to be `pub`?

---

_@zanieb reviewed on 2025-06-12 20:08_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:675 on 2025-06-12 20:08_

That last sentence is part of what confused me at https://github.com/astral-sh/uv/pull/13954/files#r2143538708

---

_@zanieb reviewed on 2025-06-12 20:09_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:804 on 2025-06-12 20:09_

Nit: Should this just be `exists()`?

---

_@zanieb reviewed on 2025-06-12 20:11_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:236 on 2025-06-12 20:11_

I think the name of this function is way too general now that it's unattached from a `ManagedPythonInstallation`. Previously, it was quite specifically "create a `bin/` link to this managed Python interpreter". Now it seems to be used for more? We should probably rename it for clarity, and update the doc?

---

_@zanieb reviewed on 2025-06-12 20:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:488 on 2025-06-12 20:57_

Why not have this be `ManagedPythonInstallations::find_minor_versions` or similar? then transform those installations into `PythonInstallationMinorVersionKey`? It feels weird to collect all installations first.

---

_@zanieb reviewed on 2025-06-12 20:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:204 on 2025-06-12 20:58_

Should this be removed? You need these later, right? I worry about this tracing regression, we have that for a reason.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:488 on 2025-06-12 20:59_

Contrary to my comment, I'm a bit confused that we're not just using https://github.com/astral-sh/uv/pull/13954/files#r2143645173 for `existing_installations`?

---

_@zanieb reviewed on 2025-06-12 20:59_

---

_@jtfmumm reviewed on 2025-06-12 21:00_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:204 on 2025-06-12 21:00_

This isn't removed. It's just been moved above

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:488 on 2025-06-12 21:04_

Haven't we potentially installed new installations by this point? The naming of `existing_installations` could be improved here

---

_@jtfmumm reviewed on 2025-06-12 21:04_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile_scenarios.rs`:425 on 2025-06-12 21:05_

Why is this here? We can't mutate the scenario files, they're generated.

---

_@zanieb reviewed on 2025-06-12 21:05_

---

_@zanieb reviewed on 2025-06-12 21:05_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:4069 on 2025-06-12 21:05_

Same as https://github.com/astral-sh/uv/pull/13954/files#r2143657006 — if you want to mutate these it needs to happen in the template.

---

_@jtfmumm reviewed on 2025-06-12 21:08_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_compile_scenarios.rs`:425 on 2025-06-12 21:08_

Sorry, I thought I commented on this. This is here to prevent the test from using the highest available patch version if a minor link directory has been installed. Our test environments seem like they're not isolated enough to be able to simulate only having certain patches installed.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:204 on 2025-06-12 21:08_

Ah I see, thanks! I think I'll need to review this file in an editor, the diff is hard to follow.

---

_@zanieb reviewed on 2025-06-12 21:08_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_install_scenarios.rs`:4069 on 2025-06-12 21:08_

Same as comment above (and for other instances)

---

_@jtfmumm reviewed on 2025-06-12 21:08_

---

_@zanieb reviewed on 2025-06-12 21:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:488 on 2025-06-12 21:10_

I see, that makes more sense. I think we've previously avoiding repeatedly reading all the installations. I find it surprising that we'd need to do that? We know what we've just installed via the `installations` variable.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:488 on 2025-06-12 21:12_

I think it seems better to operate on the subset of Python versions requested in the install? Whereas this will consider all Python installations?

---

_@zanieb reviewed on 2025-06-12 21:12_

---

_@zanieb reviewed on 2025-06-12 21:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:472 on 2025-06-12 21:16_

I'm finding it confusing that we're passing `preview` in here _and_ using `preview` to determine if the value of `upgradable`?

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:472 on 2025-06-12 21:18_

I'm just being defensive there. If preview is disabled, `upgradeable` is effectively `false`, so I was trying to avoid confusion when it's used downstream.

---

_@jtfmumm reviewed on 2025-06-12 21:18_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:488 on 2025-06-12 21:20_

The reason to consider all Python installations is to ensure we create minor version links for installations from prior to these changes to uv. But I can look at this again and see if I can combine the old existing_installations with the new ones

---

_@jtfmumm reviewed on 2025-06-12 21:20_

---

_@zanieb reviewed on 2025-06-12 21:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile_scenarios.rs`:425 on 2025-06-12 21:36_

I think that means we might want to canonicalize Python executables before using them in the test suite? There are various tests that do require specific patch versions, and rely on that `TestContext` behavior.

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:4069 on 2025-06-12 21:36_

These changes will be undone on the next scenario update, they need to go in the template if they must be here. Though we can discuss ways to make them unnecessary above.

---

_@zanieb reviewed on 2025-06-12 21:36_

---

_@jtfmumm reviewed on 2025-06-12 21:38_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_compile_scenarios.rs`:425 on 2025-06-12 21:38_

Maybe we can canonicalize them only with the `python-patch` feature 

---

_@jtfmumm reviewed on 2025-06-13 15:22_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:156 on 2025-06-13 15:22_

Updated in #14029

---

_@jtfmumm reviewed on 2025-06-13 15:22_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:748 on 2025-06-13 15:22_

Updated in #14029

---

_@jtfmumm reviewed on 2025-06-13 15:22_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:62 on 2025-06-13 15:22_

Updated in #14029

---

_@jtfmumm reviewed on 2025-06-13 15:22_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:78 on 2025-06-13 15:22_

Updated in #14029

---

_@jtfmumm reviewed on 2025-06-13 15:22_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:828 on 2025-06-13 15:22_

Updated in #14029

---

_@jtfmumm reviewed on 2025-06-13 15:22_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:804 on 2025-06-13 15:22_

Updated in #14029

---

_@jtfmumm reviewed on 2025-06-13 15:23_

---

_Review comment by @jtfmumm on `crates/uv-virtualenv/src/virtualenv.rs`:236 on 2025-06-13 15:23_

Updated in #14029

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/install.rs`:488 on 2025-06-13 15:23_

Updated in #14029, chaining existing installations with the new ones

---

_@jtfmumm reviewed on 2025-06-13 15:23_

---

_@jtfmumm reviewed on 2025-06-13 15:23_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_compile_scenarios.rs`:425 on 2025-06-13 15:23_

Updated in #14029 and removed those `uv venv` calls from the tests

---

_@jtfmumm reviewed on 2025-06-13 15:24_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_install_scenarios.rs`:4069 on 2025-06-13 15:24_

Updated in #14029

---

_@konstin reviewed on 2025-06-16 12:16_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:644 on 2025-06-16 12:16_

iitc if we get a relative path here we don't know what it would be relative too (`CWD`,  `PYTHONHOME` or something else), so we chose to make this the responsibility of the caller. We're now passing `sys_executable` and `sys_base_executable` here, which are not enforced to be absolute, so we should change this from a panic to an error.

---

_Review comment by @konstin on `crates/uv-trampoline/trampolines/trampolines/uv-trampoline-aarch64-console.exe`:1 on 2025-06-16 12:17_

Are those duplicates?

---

_Review comment by @konstin on `crates/uv/src/settings.rs`:977 on 2025-06-16 12:30_

Unused clippy allow

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:349 on 2025-06-16 12:34_

nit: assign this to a variable outside the tuple to make it easier to follow that this is a tuple; ideally, we'd also de-nest it a bit.

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:183 on 2025-06-16 13:03_

We're using the randomize-order hashset here, but `requests.first()` is special below. Can we ensure deterministic order?

---

_Review comment by @konstin on `crates/uv-python/src/installation.rs`:542 on 2025-06-16 13:36_

Can we use something ordered here? (And potentially remove the hash derives afterwards)

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:661 on 2025-06-16 13:38_

```suggestion
    /// The symlink directory (or junction on Windows).
```

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:359 on 2025-06-16 14:06_

Can you add something (cheap) that checks that we're not creating/using the new links in the non-preview `python_install`?

---

_Comment by @konstin on 2025-06-16 14:16_

I found a bad interaction between our two preview features:

```
$ uv-debug python upgrade 3.12 --preview
error: Failed to install cpython-3.12.11-linux-x86_64-gnu
  Caused by: Executable already exists at `/home/konsti/.local/bin/python3.12` but is not managed by uv; use `--force` to replace it
$ uv-debug python upgrade 3.12 --preview --force
error: unexpected argument '--force' found

  tip: to pass '--force' as a value, use '-- --force'

Usage: uv-debug python upgrade <TARGETS>...

For more information, try '--help'.
```

---

_@konstin approved on 2025-06-16 14:38_

---

_@zanieb reviewed on 2025-06-16 17:59_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:977 on 2025-06-16 17:59_

(I think this is fine, there are a bunch of copies of this. We should also just turn off that rule because we always ignore it)

---

_Comment by @jtfmumm on 2025-06-16 19:17_

> I found a bad interaction between our two preview features:
> 
> ```
> $ uv-debug python upgrade 3.12 --preview
> error: Failed to install cpython-3.12.11-linux-x86_64-gnu
>   Caused by: Executable already exists at `/home/konsti/.local/bin/python3.12` but is not managed by uv; use `--force` to replace it
> $ uv-debug python upgrade 3.12 --preview --force
> error: unexpected argument '--force' found
> 
>   tip: to pass '--force' as a value, use '-- --force'
> 
> Usage: uv-debug python upgrade <TARGETS>...
> 
> For more information, try '--help'.
> ```

I changed this (in #14084) to be a warning in the upgrade case, hinting at using `--force` with `uv python install`.

---

_@jtfmumm reviewed on 2025-06-16 19:21_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/trampolines/trampolines/uv-trampoline-aarch64-console.exe`:1 on 2025-06-16 19:21_

I've removed those ones

---

_@jtfmumm reviewed on 2025-06-17 09:11_

---

_Review comment by @jtfmumm on `crates/uv/src/settings.rs`:977 on 2025-06-17 09:11_

I created a separate PR off `main` to turn this off: #14102

---

_Label `enhancement` added by @jtfmumm on 2025-06-17 12:37_

---

_Label `preview` added by @jtfmumm on 2025-06-18 14:31_

---

_@zanieb reviewed on 2025-06-18 15:11_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1424 on 2025-06-18 15:11_

This doesn't match how we use `--preview` elsewhere, this should just show a warning, e.g.,

https://github.com/astral-sh/uv/blob/f08b27827a2cb2af4de780c2b587f816b8617e8e/crates/uv/src/commands/pip/install.rs#L134-L140

and should probably be in the implementation for consistency?

---

_@jtfmumm reviewed on 2025-06-18 17:46_

---

_Review comment by @jtfmumm on `crates/uv/src/lib.rs`:1424 on 2025-06-18 17:46_

Updated

---

_Merged by @jtfmumm on 2025-06-20 14:17_

---

_Closed by @jtfmumm on 2025-06-20 14:17_

---

_Branch deleted on 2025-06-20 14:17_

---

_Comment by @neutrinoceros on 2025-06-20 16:48_

Hi, I think I authored the oldest request for this feature (#7287) and I must say, I'm amazed by the amount of work that went into it. I had *no* idea it would be that involved, and I'm eternally grateful for your effort. Thank you so much !

---

_Comment by @callegar on 2025-06-21 09:22_

Will this also be available for the `tool` subcommand that internally relies on virtualenvs. Specifically, will it be possible to say `uv tool update -p <something>`, or `uv tool update -p <something> --all`?

Even more interesting: in case you upgrade a managed python minor version (e.g `uv python install 3.15.3` and then `uv python uninstall 3.15.2`) will it then be possible to have `uv tool update --all` update all the virtualenvs using the not-anymore-existing minor python version to the newer one?

---
