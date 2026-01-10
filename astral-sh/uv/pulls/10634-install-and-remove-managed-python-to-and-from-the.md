```yaml
number: 10634
title: Install and remove managed Python to and from the Windows Registry (PEP 514)
type: pull_request
state: merged
author: konstin
labels:
  - windows
  - preview
assignees: []
merged: true
base: main
head: konsti/pep-514
created_at: 2025-01-15T13:29:30Z
updated_at: 2025-01-23T14:13:43Z
url: https://github.com/astral-sh/uv/pull/10634
synced_at: 2026-01-10T11:45:00Z
```

# Install and remove managed Python to and from the Windows Registry (PEP 514)

---

_Pull request opened by @konstin on 2025-01-15 13:29_

## Summary

In preview mode on windows, register und un-register the managed python build standalone installations in the Windows registry following PEP 514.

We write the values defined in the PEP plus the download URL and hash. We add an entry when installing a version, remove an entry when uninstalling and removing all values when uninstalling with `--all`. We update entries only by overwriting existing values, there is no "syncing" involved.

Since they are not official builds, pbs gets a prefix. `py -V:Astral/CPython3.13.1` works, `py -3.13` doesn't.

```
$ py --list-paths                                            
 -V:3.12 *        C:\Users\Konsti\AppData\Local\Programs\Python\Python312\python.exe
 -V:3.11.9        C:\Users\Konsti\.pyenv\pyenv-win\versions\3.11.9\python.exe
 -V:3.11          C:\Users\micro\AppData\Local\Programs\Python\Python311\python.exe
 -V:3.8           C:\Users\micro\AppData\Local\Programs\Python\Python38\python.exe
 -V:Astral/CPython3.13.1 C:\Users\Konsti\AppData\Roaming\uv\data\python\cpython-3.13.1-windows-x86_64-none\python.exe
```

Registry errors are reported but not fatal, except for operations on the company key since it's not bound to any specific python interpreter.

On uninstallation, we prune registry entries that have no matching Python installation (i.e. broken entries).

The code uses the official `windows_registry` crate of the `winreg` crate.

Best reviewed commit-by-commit.

## Test Plan

We're reusing an existing system check to test different (un)installation scenarios.

---

_Label `windows` added by @konstin on 2025-01-15 13:29_

---

_Label `preview` added by @konstin on 2025-01-15 13:29_

---

_Review requested from @Gankra by @konstin on 2025-01-15 13:29_

---

_Review requested from @zanieb by @konstin on 2025-01-15 13:29_

---

_Assigned to @zanieb by @zanieb on 2025-01-15 14:50_

---

_Comment by @Gankra on 2025-01-15 14:54_

> Since the registry is global, we can't isolate this into integration tests.

It's not terribly pleasant but you can do runs-in-serial integration tests that save+restore (or clear) the relevant keys if they're vaguely easy to enumerate (e.g. if they're all under `HKCU\Software\Python\Astral`).

---

_Comment by @zanieb on 2025-01-15 14:56_

Need to review still, but could we add coverage to one of our integration tests? Like [integration test | free-threaded on windows](https://github.com/astral-sh/uv/actions/runs/12789383253/job/35652882401?pr=10634#logs) could assert that we can find it with `-py` etc.? We could create a dedicated "python install on windows" integration test too.

---

_Review comment by @Gankra on `crates/uv-python/src/installation.rs`:322 on 2025-01-15 15:11_

```suggestion
        format!("{}.{}.{}", self.major, self.minor, self.patch)
```

---

_Review comment by @Gankra on `crates/uv-python/src/lib.rs`:49 on 2025-01-15 15:12_

just double checking we indeed want Astral and not Astral.sh or AstralSh or something (assume this was already discussed)

---

_Review comment by @Gankra on `crates/uv-python/src/managed.rs`:342 on 2025-01-15 15:15_

oh interesting... is windowed python a thing *only* windows users have available? pythonw simply doesn't exist on linux/macos?

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:25 on 2025-01-15 15:22_

Interesting... it would be nice if we could consider this stuff Authoritative If Present but if the script works and is more portable, i guess it's fine... (also we can just add this stuff later so w/e)

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:66 on 2025-01-15 15:31_

love all these comments 💯 

---

_@zanieb reviewed on 2025-01-15 15:32_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:343 on 2025-01-15 15:32_

Is adding this bool here _just_ for this one use?

```
    install_path.set_value(
        "WindowedExecutablePath",
        &Value::from(&HSTRING::from(installation.executable(true).as_os_str())),
    )?;
```

It makes me a little sad to need to pass `false` everywhere. Unfortunately the logic here is fairly involved... hm.

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:79 on 2025-01-15 15:34_

💭 i wonder if citing the text of the PEP in these comments is a good idea

> If a string value named ExecutablePath exists, it must be the full path to the python.exe (or equivalent) executable. **If omitted, the environment is not executable.**

---

_@zanieb reviewed on 2025-01-15 15:35_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:296 on 2025-01-15 15:35_

Perhaps we should say "constructed" instead of "built" (here and below), I thought were were talking about building the CPython distribution itself for a second.

---

_@zanieb reviewed on 2025-01-15 15:36_

---

_Review comment by @zanieb on `crates/uv-python/src/windows_registry.rs`:149 on 2025-01-15 15:36_

```suggestion
    // Ex) CPython3.13.1
```

(presuming)

---

_Review comment by @zanieb on `crates/uv-python/src/windows_registry.rs`:167 on 2025-01-15 15:37_

nit
```suggestion
    // Store `python-build-standalone` release
```

---

_@zanieb reviewed on 2025-01-15 15:37_

---

_Comment by @zanieb on 2025-01-15 15:40_

Looks good to me! I defer to @Gankra on approval since she's more familiar with Windows.

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:98 on 2025-01-15 15:41_

This doesn't implement the PythonCore backcompat handling, is that intended / future-work?

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:104 on 2025-01-15 15:42_

Wait, do we skip it? Seems like we at least support displaying it?

---

_Comment by @zanieb on 2025-01-15 15:43_

Just wondering, what's the motivation for

>  ... plus the download URL and hash

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:146 on 2025-01-15 15:45_

(Another "this is indeed our preferred branding?" check)

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:154 on 2025-01-15 15:49_

hurts my soul that there isn't a clear delimeter (I know SystemVersion is the Real version but...)

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:154 on 2025-01-15 15:53_

Also idk if this is totally respecting this guidance of PEP514 but I view it as kind of unhinged anyway so it's fine

> It is expected that some tools will require users to type the Tag into a command line, and that the Company may be optional provided the Tag is unique across all Python installations. Short, human-readable and easy to type Tags are recommended, **and if possible, select a value likely to be unique across all other Companies.**

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:98 on 2025-01-15 15:58_

(I won't bring up or check for PythonCore special handling elsewhere, will do a second pass for it if this is indeed an issue.)

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:198 on 2025-01-15 16:04_

I would personally encode this as `installations: Option<&[ManagedPythonInstallation]>`

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:216 on 2025-01-15 16:06_

...this really feels like it should be a method, it would be really spicy to have the reader and writer diverge on this... although admittedly changing it at all is a huge hazard so... eugh.

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:199 on 2025-01-15 16:07_

oh interesting, an out-param and not a return?

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:230 on 2025-01-15 16:09_

```suggestion
                errors.push((
                    installation.key().clone(),
                    anyhow::Error::new(err)
                        .context("Failed to clear registry entries under HKCU:\\{python_entry}"),
                ));
```

* More correct/useful to indicate we're trying to edit HKCU:\some\registery\key
* probably want to mention the full python_entry here and not astral_key

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:230 on 2025-01-15 16:14_

(There's probably a few other places where the HKCU thing also applies, I can go back and mark them all if you want, but for now I'm not)

---

_Review comment by @Gankra on `crates/uv/src/commands/python/install.rs`:458 on 2025-01-15 16:26_

i'm going to assume this is a direct cut-paste and not look at it unless you tell me otherwise

---

_Review comment by @Gankra on `crates/uv/src/commands/python/install.rs`:346 on 2025-01-15 16:27_

(i wonder if it would be better to pass in preview and have these functions decide whether they're enabled... nbd tho)

---

_@Gankra approved on 2025-01-15 16:29_

Looks good. Biggest concern is the lack of special handling for the "official python install" special cases described in the PEP.

---

_@zanieb reviewed on 2025-01-15 16:35_

---

_Review comment by @zanieb on `crates/uv-python/src/lib.rs`:49 on 2025-01-15 16:35_

Not discussed as far as I know, but that's my preference (Astral).

We're technically "Astral Software Inc", not "Astral.sh"

---

_@zanieb reviewed on 2025-01-15 16:35_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:342 on 2025-01-15 16:35_

Yeah it's not needed elsewhere / does not exist.

---

_@konstin reviewed on 2025-01-17 11:14_

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:342 on 2025-01-17 11:14_

To add more context, windows is the only platform where process would open a terminal by default, so it's the only platform where we need windowed builds. Windows also has a completely different binary naming scheme.

---

_@konstin reviewed on 2025-01-17 11:18_

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:343 on 2025-01-17 11:18_

I bailed on rewriting the entire logic here, not sure either.

---

_Review comment by @konstin on `crates/uv-python/src/windows_registry.rs`:154 on 2025-01-17 11:23_

I'm assuming this is for deployments of custom interpreters (think jython, pypy, etc.), and doesn't account for cases like us who are shipping almost pristine cpython and pypy.

---

_@konstin reviewed on 2025-01-17 11:23_

---

_@konstin reviewed on 2025-01-17 11:36_

---

_Review comment by @konstin on `crates/uv-python/src/windows_registry.rs`:98 on 2025-01-17 11:36_

We don't support those python versions anymore, so we are not affected

---

_Review comment by @konstin on `crates/uv-python/src/windows_registry.rs`:104 on 2025-01-17 11:39_

(this code only moved, but didn't change) We query interpreters where version is `None` through a python script later.

---

_@konstin reviewed on 2025-01-17 11:40_

---

_@konstin reviewed on 2025-01-17 11:55_

---

_Review comment by @konstin on `crates/uv-python/src/windows_registry.rs`:25 on 2025-01-17 11:55_

There's a cross-platform solution that we'd want in https://github.com/python/cpython/issues/107956

---

_@konstin reviewed on 2025-01-17 12:37_

---

_Review comment by @konstin on `crates/uv-python/src/windows_registry.rs`:199 on 2025-01-17 12:37_

We're collecting different sources of non-fatal errors in this vec and report them in the end.

---

_@konstin reviewed on 2025-01-17 12:41_

---

_Review comment by @konstin on `crates/uv-python/src/windows_registry.rs`:216 on 2025-01-17 12:41_

good catch

---

_Converted to draft by @konstin on 2025-01-17 13:02_

---

_@Gankra reviewed on 2025-01-17 13:25_

---

_Review comment by @Gankra on `crates/uv-python/src/windows_registry.rs`:36 on 2025-01-17 13:25_

Oh I just realized, it's not obvious to me that the current approach actually applies the "CURRENT_USER should shadow LOCAL_MACHINE if they overlap" rule? It seems like the logic here throws out that information completely. (I don't think this has to be a blocker, but it should at least be noted that this is "future work / a looming issue")?

> Tools that list every installed environment may choose to include those even where the Company-Tag pairs match. They should ensure users can easily identify whether the registration was per-user or per-machine, and which registration has the higher priority.
> 
> Tools that aim to select a single installed environment from all registered environments based on the Company-Tag pair, such as the py.exe launcher, **should always select the environment registered in HKEY_CURRENT_USER when than the matching one in HKEY_LOCAL_MACHINE**.


---

_@konstin reviewed on 2025-01-17 13:33_

---

_Review comment by @konstin on `crates/uv/src/commands/python/install.rs`:458 on 2025-01-17 13:33_

yes the first two commits are just moving code around

---

_@konstin reviewed on 2025-01-17 15:47_

---

_Review comment by @konstin on `crates/uv-python/src/windows_registry.rs`:36 on 2025-01-17 15:47_

The way its written sounds like its targeted at a different case than ours: When a user says `py -V:Astral/CPython3.13.1`, it should use HKEY_CURRENT_USER over HKEY_LOCAL_MACHINE if both have `Astral\CPython3.13.1`. In uv's discovery, we only support asking of an interpreter kind and a version or version range, and we'll return the first Python we find matching. If a user has the case that they have multiple installations of the same version and they have a specific preference, they can use an absolute path (this should be an edge case). By the order in the list we should still prefer HKEY_CURRENT_USER over HKEY_LOCAL_MACHINE if the version is the same, i added a comment about it.

---

_Renamed from "Install managed Python into the Windows Registry (PEP 514)" to "Install and remove managed Python into the Windows Registry (PEP 514)" by @konstin on 2025-01-21 10:41_

---

_Renamed from "Install and remove managed Python into the Windows Registry (PEP 514)" to "Install and remove managed Python to and from the Windows Registry (PEP 514)" by @konstin on 2025-01-21 10:42_

---

_Marked ready for review by @konstin on 2025-01-21 10:42_

---

_@zanieb reviewed on 2025-01-21 23:13_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1609 on 2025-01-21 23:13_

Any particular reason you choose the 3.10 Windows x86 job for this? I think this makes more sense as a dedicated integration test job — this job is specific to our discovery of system-installed Python interpreters on x86 systems.

---

_@zanieb approved on 2025-01-21 23:13_

---

_Merged by @konstin on 2025-01-23 14:13_

---

_Closed by @konstin on 2025-01-23 14:13_

---

_Branch deleted on 2025-01-23 14:13_

---
